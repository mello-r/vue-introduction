<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button class="button" @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
.button {
  background-color: grey;
  padding: 0.25rem 1rem;
  border-radius: 0.25rem;
}
</style>