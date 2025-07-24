<template>
    <div class="container">
        <form @submit.prevent="editingEmployee ? updateEmployee() : addEmployee()">
            <div class="form-group">
                <label for="name">Enter First Name</label>
                <input type="text" name="firstName" class="form-control" v-model="newEmployee.firstName">
            </div>

            <div class="form-group">
                <label for="name">Enter Last Name</label>
                <input type="text" name="lastName" class="form-control" v-model="newEmployee.lastName">
            </div>

            <div class="form-group">
                <label for="name">Enter Email</label>
                <input type="text" name="email" class="form-control" v-model="newEmployee.email">
            </div>

            <div class="form-group">
                <label for="name">Enter Contact No</label>
                <input type="text" name="contactNo" class="form-control" v-model="newEmployee.contactNo">
            </div>

            <div class="form-group">
                <label for="name">Enter City</label>
                <input type="text" name="city" class="form-control" v-model="newEmployee.city">
            </div>

            <div class="form-group">
                <label for="name">Enter Address</label>
                <input type="text" name="address" class="form-control" v-model="newEmployee.address">
            </div>

            <button class="btn btn-primary btn-md mt-4">{{ editingEmployee ? 'Update' : 'Add' }}</button>
            <button v-if="editingEmployee" class="btn btn-secondary mt-4 ms-2" @click="cancelEdit" type="button">Cancel</button>
        </form>
        <br>
        <table class="table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>Email</th>
                    <th>Contact No</th>
                    <th>City</th>
                    <th>Address</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="emp in employees" :key="emp.employeeId">
                    <td>{{ emp.employeeId }}</td>
                    <td>{{ emp.firstName }}</td>
                    <td>{{ emp.lastName }}</td>
                    <td>{{ emp.email }}</td>
                    <td>{{ emp.contactNo }}</td>
                    <td>{{ emp.city }}</td>
                    <td>{{ emp.address }}</td>
                    <td>
                        <button class="btn btn-primary btn-md me-2" @click="editEmployee(emp)">Edit</button>
                        <button class="btn btn-danger btn-md" @click="deleteEmployee(emp.employeeId)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup lang="ts">
    import { onMounted, ref } from 'vue';
    import axios from 'axios';

    interface Employee {
        employeeId: number;
        firstName: string;
        lastName: string;
        email: string;
        contactNo: string;
        city: string;
        address: string;
    }

    const employees = ref<Employee[]>([]);
    const newEmployee = ref<Omit<Employee, 'employeeId'>>({
        firstName: '',
        lastName: '',
        email: '',
        contactNo: '',
        city: '',
        address: ''
    });
    const editingEmployee = ref<Employee | null>(null);

    
    const fetchEmployees = async () => {
        try {
            const res = await axios.get('http://localhost:8000/api/employees');
            employees.value = res.data;
        } catch (error) {
            console.error('Error fetching employees:', error);
        }
    };

    const addEmployee = async () => {
        try {
            const res = await axios.post('http://localhost:8000/api/employees', newEmployee.value);
            employees.value.push(res.data);
            resetForm();
        } catch (error) {
            console.error('Error adding employee:', error);
        }
    };

    const editEmployee = (emp: Employee) => {
        editingEmployee.value = { ...emp };
        Object.assign(newEmployee.value, emp);
    };

    const updateEmployee = async () => {
        if (!editingEmployee.value) return;
        try {
            const res = await axios.put(`http://localhost:8000/api/employees/${editingEmployee.value.employeeId}`, newEmployee.value);
            const idx = employees.value.findIndex(e => e.employeeId === editingEmployee.value!.employeeId);
            if (idx !== -1) {
            employees.value[idx] = res.data;
            }
            resetForm();
            editingEmployee.value = null;
        } catch (error) {
            console.error('Error updating employee:', error);
        }
    };

    const deleteEmployee = async (id: number) => {
        try {
            await axios.delete(`http://localhost:8000/api/employees/${id}`);
            employees.value = employees.value.filter(e => e.employeeId !== id);
        } catch (error) {
            console.error('Error deleting employee:', error);
        }
    };

    const cancelEdit = () => {
        editingEmployee.value = null;
        resetForm();
    };

    const resetForm = () => {
        newEmployee.value = {
            firstName: '',
            lastName: '',
            email: '',
            contactNo: '',
            city: '',
            address: ''
        };
    };
    
    onMounted(fetchEmployees);
</script>

<style lang="css" scoped>

</style>