<template>
  <div class="container-fluid" style="background-color: lightgray; padding: 10px">
    <div class="row mb-3">
      <div class="col-md-4">
        <a
          class="navbar-brand"
          href="https://cubesplatform.com/cubes-platform"
          target="_blank"
          ><img class="logo" :src="logo"
        /></a>
      </div>
      <div class="col-md-4">
        <select
          class="form-select"
          @change="(event) => $emit('onselect', JSON.parse(event.target.value))"
        >
          <option value='{"data":"employees", "schema":"empSchema"}'>Employees</option>
          <option value='{"data":"cars", "schema":"carSchema"}'>Cars</option>
        </select>
      </div>

      <div class="col-md-4">
        <div class="input-group mb-4">
          <input
            v-model="text"
            type="text"
            class="form-control"
            placeholder="Search record"
            @input="$emit('onInput', text)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import logo from "@/assets/logo.png";
import { ref } from "vue";

const emit = defineEmits(["onSelect", "onInput"]);
defineProps({
  schema: {
    type: Array,
    required: true,
  },
});

const text = ref("");
</script>
<style scoped>
.col-md-3 {
  align-content: center;
}

.form-control {
  width: 40%;
}
</style>
