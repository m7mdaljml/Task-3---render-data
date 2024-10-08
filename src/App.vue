<template>
  <v-navbar :schema="schema" @onselect="changeData" @onInput="selectUser" />
  <v-table
    :data="searchResult"
    :schema="schema"
    :first="first"
    :end="end"
    @Delete="deleteSelected"
  />
  <pagination
    :data="searchResult"
    @updateSlice="updateSlice"
    :numOfPagination="9"
    :numOfRows="numOfRows"
  />
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import VTable from "@/components/table.vue";
import VNavbar from "@/components/navbar.vue";
import pagination from "@/components/pagination.vue";
import axios from "axios";

const cars = ref([]);
const carSchema = ref(["id", "car_make", "car_model_year", "car_vin"]);
const employees = ref([]);
const empSchema = ref(["id", "first_name", "last_name", "email", "gender", "ip_address"]);
const data = ref([]);
const schema = ref([]);
const searchValue = ref("");
const searchKey = ref("");
const first = ref(0);
const numOfRows = ref(10);
const end = ref(numOfRows.value);
const searchResult = computed(() => {
  if (!searchValue.value) {
    return data.value;
  } else {
    return data.value.filter((item) =>
      String(item[searchKey.value]).toLowerCase().includes(searchValue.value)
    );
  }
});

const getData = async () => {
  try {
    const carRes = await axios.get("/cars.json");
    cars.value = carRes.data;
    const employeesRes = await axios.get("/employees.json");
    employees.value = employeesRes.data;

    data.value = employees.value;
    schema.value = empSchema.value;
  } catch (err) {
    console.log(err);
  }
};

const changeData = (obj) => {
  if (obj.data === "employees") {
    data.value = employees.value;
    schema.value = empSchema.value;
  } else if (obj.data === "cars") {
    data.value = cars.value;
    schema.value = carSchema.value;
  }
};
const deleteSelected = (arr) => {
  if (arr.length > 0) {
    const confirmVal = confirm("Are you sure to delete selected Items ?");
    if (confirmVal) {
      data.value = data.value.filter((item) => !arr.includes(item.id));
    }
  }
};
const selectUser = (text) => {
  searchValue.value = text.toLowerCase();
  searchKey.value = schema.value[1];
};
const SearchResult = computed(() => {
  if (!SearchValue.value) {
    return data.value;
  } else {
    return data.value.filter((item) =>
      String(item[SearchKey.value]).toLowerCase().includes(SearchValue.value)
    );
  }
});

const updateSlice = ({ first: newFirst, end: newEnd }) => {
  first.value = newFirst;
  end.value = newEnd;
};

onMounted(async () => {
  await getData();
});
</script>

<style scoped></style>
