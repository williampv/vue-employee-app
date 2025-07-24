<template>
    <div class="container">

        <form>
            <div class="form-group">
                <label for="name">Enter Name</label>
                <input type="text" name="name" class="form-control" >
            </div>

            <div class="form-group">
                <label for="name">Enter Email</label>
                <input type="text" name="email" class="form-control" >
            </div>

            <button class="btn btn-primary btn-sm mt-4">Submit</button>
        </form>
        <br>
        <table class="table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Name</th>
                    <th>Email</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="emp in employees" :key="emp.employeeId">
                    <td>{{ emp.employeeId }}</td>
                    <td>{{ emp.firstName }}</td>
                    <td>{{ emp.email }}</td>
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
        email: string;
    }

    const employees = ref<Employee[]>([]);

    onMounted(() => {
        axios.get('http://localhost:8000/api/employees')
            .then(response => {
                employees.value = response.data;
                console.log('Employees fetched:', employees.value);
            })
            .catch(error => {
                console.error('Error fetching employees:', error);
            });
    });
</script>

<style lang="css" scoped>

</style>