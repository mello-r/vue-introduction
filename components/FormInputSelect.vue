<script setup>
  import { ref } from "vue"
  const selected = ref([])
</script>

<template>
  <div>
    <div>Selected: {{ selected }}</div>

    <select multiple v-model="selected" class="w-24">
        <option :value="{ code: 'A' }">A</option>
        <option :value="{ code: 'B' }">B</option>
        <option :value="{ code: 'C' }">C</option>
    </select>
  </div>
</template>