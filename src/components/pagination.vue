<template>
  <template v-if="props.data.length > props.numOfRows">
    <ul class="pagination justify-content-center gap-2">
      <li class="page-item" :class="{ disabled: CurrPage === 1 }">
        <span class="page-link" @click="decrement(1)">Prev</span>
      </li>

      <template v-for="page in pagesToShow" :key="page">
        <li class="page-item" :class="{ active: page === CurrPage }">
          <a class="page-link" @click="goToPage(page)">{{ page }}</a>
        </li>
      </template>

      <li class="page-item" :class="{ disabled: CurrPage >= totalPages }">
        <a class="page-link" @click="increment(1)">Next</a>
      </li>
    </ul>
  </template>
</template>

<script setup>
import { ref, watch, computed } from "vue"

const emit = defineEmits(["updateSlice"])
const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
  numOfPagination: {
    type: Number,
    required: false,
  },
  numOfRows: {
    type: Number,
    required: true,
  },
})
if (props.numOfPagination == "") {
  props.numOfPagination = 8
}
const PrevPage = ref(0)
const CurrPage = ref(1)
const NextPage = ref(2)
const first = ref(0)
const end = ref(props.numOfRows)

const totalPages = computed(() => Math.ceil(props.data.length / props.numOfRows))

const pagesToShow = computed(() => {
  const range = Math.floor(props.numOfPagination / 2)
  let start = Math.max(1, CurrPage.value - range)
  let endPage = start + props.numOfPagination - 1

  if (endPage > totalPages.value) {
    endPage = totalPages.value
    start = Math.max(1, endPage - props.numOfPagination + 1)
  }

  return Array.from({ length: endPage - start + 1 }, (_, i) => i + start)
})

const increment = (key) => {
  if (CurrPage.value < totalPages.value) {
    first.value += props.numOfRows * key
    end.value += props.numOfRows * key
    CurrPage.value += key
    PrevPage.value += key
    NextPage.value += key
    emit("updateSlice", { first: first.value, end: end.value })
  }
}

const decrement = (key) => {
  if (CurrPage.value > 1) {
    first.value -= props.numOfRows * key
    end.value -= props.numOfRows * key
    CurrPage.value -= key
    PrevPage.value -= key
    NextPage.value -= key
    emit("updateSlice", { first: first.value, end: end.value })
  }
}

const goToPage = (page) => {
  const key = page - CurrPage.value
  increment(key)
}

watch(
  () => props.numOfRows,
  (newNumOfRows) => {
    end.value = Number(first.value) + Number(newNumOfRows)
    emit("updateSlice", { first: first.value, end: end.value })
  }
)
</script>

<style scoped>
li {
  cursor: pointer;
  user-select: none;
  width: 50px;
  height: 50px;
  text-align: center;
}

li.disabled {
  pointer-events: none;
  opacity: 0.5;
}
</style>
