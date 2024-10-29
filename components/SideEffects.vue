<script setup lang="ts">
import { ref, watch } from 'vue'

const question = ref('')
const answer = ref('')
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    await new Promise(r => setTimeout(r, 1000))
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    } finally {
      loading.value = false
    }
  }
})
</script>

<template>
  <div>
    <p>Stelle eine Ja oder Nein Frage: <input v-model="question" :disabled="loading" /></p>
    <p>{{ answer }}</p>
  </div>
</template>