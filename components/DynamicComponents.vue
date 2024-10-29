<script setup>
import Home from './Home.vue'
import Posts from './Posts.vue'
import Archive from './Archive.vue'
import { ref } from 'vue'
 
const currentTab = ref('Home')

const tabs = {
  Home,
  Posts,
  Archive
}
</script>

<template>
  <div class="demo">
    <button
       v-for="(_, tab) in tabs"
       :key="tab"
       :class="['tab-button', { active: currentTab === tab }]"
       @click="currentTab = tab"
     >
      {{ tab }}
    </button>
	<component :is="tabs[currentTab]" class="tab"></component>
  </div>
</template>

<style scoped>
.demo {
  font-family: sans-serif;
}

.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #1b1b1b;
}.active {
  background: #1b1b1b;
}
.tab {
  border: 1px solid #ccc;
  padding: 10px;
}
</style>