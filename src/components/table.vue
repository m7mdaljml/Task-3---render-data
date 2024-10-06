<template>
    <div class="container mt-4">
        <div class="table-responsive">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th v-for="(key, index) in schema" :key="index">{{ key }}</th>
                        <th colspan="2">Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in data.slice(first, end)" :key="item.id">
                        <td v-for="(key, index) in schema" :key="index">{{ item[key] }}</td>
                        <td>
                            <button class="btn btn-outline-secondary btn-sm me-2">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                    class="bi bi-pencil-square" viewBox="0 0 16 16">
                                    <path
                                        d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z" />
                                    <path fill-rule="evenodd"
                                        d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5z" />
                                </svg>
                            </button>
                            <button class="btn btn-outline-danger btn-sm">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                    class="bi bi-trash3" viewBox="0 0 16 16">
                                    <path
                                        d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5M11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47M8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5" />
                                </svg>
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <ul class="pagination justify-content-center">
            <li class="page-item" :disabled="end < 10">
                <span class="page-link" @click="decrement()" >Previous</span>
            </li>
            <li class="page-item" v-if="PrevPage != 0"><a class="page-link">{{ PrevPage }}</a></li>
            <li class="page-item active" aria-current="page">
                <span class="page-link">{{ CurrPage }}</span>
            </li>
            <li class="page-item" ><a class="page-link" >{{ NextPage }}</a></li>
            <li class="page-item">
                <a class="page-link" @click="increment()" >Next</a>
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { defineProps, ref, computed } from 'vue'
const PrevPage = ref(0)
const CurrPage = ref(1)
const NextPage = ref(2)
const props = defineProps({
    data: {
        type: Array,
        required: true
    },
    schema: {
        type: Array,
        required: true
    }
})
let first = ref(0)
let end = ref(10)

const increment = () => {
    if (end.value <= 990) {
        first.value += 10
        end.value += 10
        PrevPage.value++
        CurrPage.value++
        NextPage.value++

    }
}
const decrement = () => {
    if (first.value >= 10) {
        first.value -= 10
        end.value -= 10
        PrevPage.value--
        CurrPage.value--
        NextPage.value--
    }
}
</script>
<style scoped>
.table {
    border: 1px solid lightgray;
}
</style>
