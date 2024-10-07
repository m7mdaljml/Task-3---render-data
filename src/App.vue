<template>
  <v-navbar :schema="schema" @onSelect="ChangeData" @onInput="SelectUser"/>
  <v-table :data="SearchResult" :schema="schema" @Delete="DeleteSelected"/>
  <table></table>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import VTable from '@/components/table.vue'
import VNavbar from '@/components/navbar.vue'
import axios from 'axios'

const Cars = ref([])
const CarSchema = ref(['id', 'car_make', 'car_model_year', 'car_vin'])
const Employees = ref([])
const EmpSchema = ref(['id', 'first_name', 'last_name', 'email', 'gender', 'ip_address'])
const data = ref([])
const schema = ref([])
const SearchValue = ref('')
const SearchKey = ref('')

const GetData = async () => {
  try {
    const CarRes = await axios.get('cars.json')
    Cars.value = CarRes.data
    const EmployeesRes = await axios.get('employees.json')
    Employees.value = EmployeesRes.data

    data.value = Employees.value
    schema.value = EmpSchema.value
  } catch (err) {
    console.log(err)
  }
}

const ChangeData = (obj) => {
  if (obj.data === 'Employees') {
    data.value = Employees.value
    schema.value = EmpSchema.value
  } else if (obj.data === 'Cars') {
    data.value = Cars.value
    schema.value = CarSchema.value
  }
}
const DeleteSelected = (arr) => {
  if(arr.length > 0){
    const confirmVal = confirm("Are you sure to delete selected Items ?")
    if(confirmVal){
      data.value = data.value.filter((item) => !arr.includes(item.id));
    }
  }
}
const SelectUser = (text,key) => {
  SearchValue.value = text.toLowerCase()
  if(!key)
  SearchKey.value = 'id'
  else
  SearchKey.value = key
}

const SearchResult = computed(() => {
  if (!SearchValue.value) {
    return data.value
  }
  else{
    return data.value.filter(item =>
      String(item[SearchKey.value]).toLowerCase().includes(SearchValue.value)
    )
  }
})

onMounted(async () => {
  await GetData()
})
</script>

<style scoped>

</style>
