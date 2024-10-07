<template>
  <v-navbar :schema="schema" @onselect="changeData" @onInput="selectUser"/>
  <v-table :data="searchResult" :schema="schema" @Delete="deleteSelected"/>
  <table></table>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import VTable from '@/components/table.vue'
import VNavbar from '@/components/navbar.vue'
import axios from 'axios'

const cars = ref([])
const carSchema = ref(['id', 'car_make', 'car_model_year', 'car_vin'])
const employees = ref([])
const empSchema = ref(['id', 'first_name', 'last_name', 'email', 'gender', 'ip_address'])
const data = ref([])
const schema = ref([])
const searchValue = ref('')
const searchKey = ref('')

const searchResult = computed(() => {
  if (!searchValue.value) {
    return data.value
  }
  else{
    return data.value.filter(item =>
      string(item[searchKey.value]).toLowercase().includes(searchValue.value)
    )
  }
})

const GetData = async () => {
  try {
    const carRes = await axios.get('cars.json')
    cars.value = carRes.data
    const employeesRes = await axios.get('employees.json')
    employees.value = employeesRes.data

    data.value = employees.value
    schema.value = empSchema.value
  } catch (err) {
    console.log(err)
  }
}

const changeData = (obj) => {
  if (obj.data === 'employees') {
    data.value = employees.value
    schema.value = empSchema.value
  } else if (obj.data === 'cars') {
    data.value = cars.value
    schema.value = carSchema.value
  }
}
const deleteSelected = (arr) => {
  if(arr.length > 0){
    const confirmVal = confirm("Are you sure to delete selected Items ?")
    if(confirmVal){
      data.value = data.value.filter((item) => !arr.includes(item.id));
    }
  }
}
const selectUser = (text,key) => {
  searchValue.value = text.toLowercase()
  if(!key)
  searchKey.value = 'id'
  else
  searchKey.value = key
}

onMounted(async () => {
  await GetData()
})
</script>

<style scoped>

</style>
