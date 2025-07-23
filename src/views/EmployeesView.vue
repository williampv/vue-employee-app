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
                <!-- <tr *ngFor="let emp of employees">
                    <td>{{ emp.id }}</td>
                    <td>{{ emp.name }}</td>
                    <td>{{ emp.email }}</td>
                </tr> -->
            </tbody>
        </table>
    </div>
</template>

<script setup lang="ts">
    
</script>

<style lang="scss" scoped>

</style>