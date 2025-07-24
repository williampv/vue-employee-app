<template>
    <div class="container">
        <form>
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

            <button class="btn btn-primary btn-md mt-4" @click="addEmployee">Submit</button>
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
                        <button class="btn btn-primary btn-md me-2">Edit</button>
                        <button class="btn btn-danger btn-md">Delete</button>
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
    const newEmployee = ref({
        firstName: '',
        lastName: '',
        email: '',
        contactNo: '',
        city: '',
        address: ''
    });

    onMounted(() => {
        fetchEmployees();
    });

    const fetchEmployees = () => {
        axios.get('http://localhost:8000/api/employees')
            .then(response => {
                employees.value = response.data;
            })
            .catch(error => {
                console.error('Error fetching employees:', error);
            });
    };

    const addEmployee = () => {
        axios.post('http://localhost:8000/api/employees', newEmployee.value)
            .then(response => {
                employees.value.push(response.data);
                newEmployee.value = {
                    firstName: '',
                    lastName: '',
                    email: '',
                    contactNo: '',
                    city: '',
                    address: ''
                };
            })
            .catch(error => {
                console.error('Error adding employee:', error);
            });
    };

</script>

<style lang="css" scoped>

</style>