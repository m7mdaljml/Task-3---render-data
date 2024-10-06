<template>

    <div class="container-fluid" style="background-color: lightgray;padding:10px;">
        <div class="row mb-3">
            <div class="col-md-4">
                <a class="navbar-brand" href="https://cubesplatform.com/cubes-platform" target="_blank"><img
                        class="logo" :src="logo"></a>
            </div>
            <div class="col-md-4">
                <select class="form-select" @change="event => $emit('onSelect', JSON.parse(event.target.value))">
                    <option value='{"data":"Employees", "schema":"EmpSchema"}'>Employees</option>
                    <option value='{"data":"Cars", "schema":"CarSchema"}'>Cars</option>
                </select>
            </div>
            <div class="col-md-4">
                <div class="input-group mb-4">
                    <input type="text" class="form-control" placeholder="Search record">
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import logo from '@/assets/logo.png'
import { ref } from 'vue'
const emit = defineEmits([
    'onSelect',
])
defineProps({
    schema: {
        type: Array,
        required: true
    }
})
</script>
<style scoped>
.col-md-3 {
    align-content: center;
}
.form-control{
    width:40%;
}
</style>
