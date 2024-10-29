---
theme: apple-basic
title: Vue.js - Nutzung, Unterschiede und sein Ecosystem
lineNumbers: true

transition: fade-out
---

# Vue.js

Nutzung, Eigenheiten und sein Ökosystem

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/mello-r/vue-introduction" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<style>
  .slidev-layout {
    --uno: h-full flex flex-col justify-center
  }
  h1 {
    --uno: text-6xl font-700 leading-20
  }
  h1 + p {
    --uno: font-700 -mt-4 text-2xl;
  }
</style>

---
transition: fade-out
layout: center
---

# Disclaimer

<!--
Kein Experte
Nur in hobby Projekten
Überblick -> viel Inhalt dafür nicht sehr detailliert
Entschuldige mich für Denglisch 
-->

---
transition: fade-out
---

# Wie sieht Vue aus?


```vue{all|4|8|12-16|8}
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
```

<Button class="mt-2" />

<!--
Kurzer Einblick detailliert später
Button benutzen
-->

---
transition: fade
layout: quote
---

# API Style - Options API

"Go with Options API if you are not using build tools, or plan to use Vue primarily in low-complexity scenarios, e.g. progressive enhancement."

---
transition: fade
layout: quote
---

# API Style - Composition API

"Go with Composition API + Single-File Components if you plan to build full applications with Vue."

<!-- 
Präsentation nur Composition API
-->

---
transition: fade-out
---

# Single File Components

````md magic-move
```vue
<script>
</script>

<template>
</template>

<style>
</style>
```

```vue
<script>
</script>

<template>
</template>

<style>
</style>

<i18n>
</i18n>
```

```vue
<script>
</script>

<template>
</template>

<style lang="scss">
</style>

<i18n>
</i18n>
```

```vue
<script setup>
  const { t } = useI18n({ ... })
</script>

<template>
<p>{{ t('hello') }}</p>
</template>

<style lang="scss">
</style>

<i18n lang="json">
{
  "en": {
    "hello": "Hello world!"
  },
  "de": {
    "hello": "Hallo Welt!"
  }
}
</i18n>
```
````

<!--
Vue template valides HTML
SFC ist die empfohlen Variante Vue Komponenten zu schreiben
Benötigt Build Tool
-->

---
transition: fade-out
---

# Vue ohne Build Tool

````md magic-move
```html {all|5|15|17-22|8-12}
<!doctype html>
<html>
<head>
  <title>Vue ohne Build Tool</title>
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>
  <div id="app">
    <button @click="count++">
      Count is: {{ count }}
    </button>
  </div>
</body>
<script>
const { createApp, ref } = Vue

createApp({
  setup() {
    const count = ref(0)
    return { count }
  }
}).mount('#app')
</script>
</html>
```
```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Vue ohne Build Tool</title>
  <script src="https://unpkg.com/petite-vue" defer init></script>
</head>
<body>
  <div v-scope="{ count: 0 }">
    <button @click="count++">
      Count is: {{ count }}
    </button>
  </div>
</body>
</html>
```
````

---
transition: fade
---

# Reactive State

ref()

````md magic-move
```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>
```

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

console.log(count.value) // 0
</script>
```

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

console.log(count.value) // 0

count.value++
console.log(count.value) // 1
</script>
```

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

console.log(count.value) // 0

count.value++
console.log(count.value) // 1
</script>

<template>
  <!-- Automatisch Unrefed -->
  <div>{{ count }}</div>
</template>
```

```vue
<script>
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    console.log(count.value) // 0

    count.value++
    console.log(count.value) // 1

    return { count }
  }
}

</script>

<template>
  <!-- Automatisch Unrefed -->
  <div>{{ count }}</div>
</template>
```
````

<!-- 
ref -> wrapped in ref Objekt mit einer property value welche mutable ist
-->

---
transition: fade-out
---

# Reactive State

reactive()

```js
const state = reactive({ count: 0 })

console.log(state.count) // 0

state.count++
console.log(state.count) // 1
```

<v-click>

Limitationen

````md magic-move
```js
const state = reactive({ count: 0 })

state = { count: 1 }
console.log(state.count) // 0

let { count } = state
count++
console.log(state.count) // 0

// reactivity verloren
callSomeFunction(state.count)
```
```js
const state = ref({ count: 0 })

state.value = { count: 1 }
console.log(state.value.count) // 1

let { count } = toRefs(state.value)
count.value++
console.log(state.value.count) // 2

// reactivity bleibt
callSomeFunction(state.value.count)
```
````
</v-click>

<!-- 
reactive -> nur Objekt wrapped mit JavaScript Proxy
ref -> nutzt reactive für verschachtelte Objekte
ref -> präferieren 
-->

---
transition: fade-out
---

# Computed State

````md magic-move
```js
const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed(() => {
  return firstName.value + ' ' + lastName.value
})
```
```js
const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // Nicht empfohlen
  set(newValue) {
    names = newValue.split(' ')
    firstName.value = names[0]
    lastName.value = names[1]
  }
})
```
````

<!-- 
Änderung sollte auf den source state gemacht werden
-->

---
transition: fade-out
---

# Data Binding

```vue-html
<!-- Data binding "Mustache" syntax -->
<span>Count is: {{ count }}</span>
```

<div class="space-y-2">
<v-clicks>

````md magic-move
```vue-html
<!-- Attribut binding -->
<div v-bind:id="id"></div>
```

```vue-html
<!-- Shorthand Attribut binding -->
<div :id="id"></div>
```

```vue-html
<!-- Shorthand Attribut binding (ab v3.4) -->
<div :id></div>
```
````

```vue
<!-- Multiple Attribut binding -->
<script setup>
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper',
  style: 'background-color:green'
}
</script>
<template>
  <div v-bind="objectOfAttrs"></div>
</template>
```

```vue-html
<!-- JavaScript Expressions -->
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<button :disabled="age < 18">Registrieren</button>
```
</v-clicks>
</div>

<!-- 
Vue template valides HTML
-->

---
transition: fade
---

# Directives

```vue
<!-- Conditional rendering -->
<p v-if="age < 13">Du bist noch ein Kind</p>
<p v-else-if="age < 18">Du bist ein Teenager</p>
<p v-else>Du bist erwachsen</p>
```

<div class="space-y-2">
<v-clicks>

```vue
<!-- Conditional visibility -->
<p v-show="seen">Ich werde angezeigt wenn 'seen' truthy ist</p>
```

````md magic-move
```vue
<!-- Lists -->
<div v-for="item in items">
  {{ item.text }}
</div>
```
```vue
<!-- Lists -->
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```
````

```vue
<!-- List Range -->
<span v-for="n in 10">{{ n }}</span>
```
</v-clicks>
</div>

<!--
Directives mächtig
v-bind von voriger folie ein directive
v-show immer gerendert mit css versteckt
v-for Range 1 bis 10 und nicht 0 bis 10
-->

---
transition: fade
---

# Directives

````md magic-move
```vue
<!-- Events -->
<button v-on:[eventName]="count++"> ... </button>
```
```vue
<!-- Events -->
<button v-on:click="count++"> ... </button>
```
```vue
<!-- Events Shorthand -->
<button @click="count++"> ... </button>
```
````

<div class="space-y-2">
<v-clicks>

```vue
<!-- Events Method Handler -->
<button @click="onButtonClick"> ... </button>
```

```vue
<!-- Modifier -->
<form @submit.prevent="submit"> ... </form>
```

```vue
<!-- Key Modifier -->
<input @keyup.enter="submit" />
```

```vue
<!-- Andere Integrierte Directives -->
v-text, v-html, v-model, v-slot, v-pre, v-once, v-memo, v-cloak

<!-- Andere Integrierte Modifier -->
.stop, .prevent, .self, .capture, .once, .passive, ...

<!-- Andere Integrierte Key Modifier -->
.enter, .tab, .delete, .esc, .space, .up, .down, .left, .right, ...
```

</v-clicks>
</div>

---
transition: fade-out
---

# Directives

<div class="flex justify-center items-center">

![alt text](./assets/directive.png)
</div>

---
transition: fade-out
---

# Form Inputs


````md magic-move
```vue
<template>
  <input :value="text" @input="event => text = event.target.value">

  <input type="checkbox" :checked="accepted" @change="_ => accepted = !accepted">

  <input type="radio" :checked="accepted" @change="_ => accepted = !accepted">

  <select :value="selected" @change="event => selected = event.target.value">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </selected>
</template>
```
```vue
<template>
  <input v-model="text">
  
  <input type="checkbox" v-model="accepted">

  <input type="radio" v-model="accepted">

  <select v-model="selected">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </selected>
</template>
```
````

---
transition: fade
---

# Form Input Checkbox

```vue
<script setup>
  const checkedNames = ref([])
</script>

<template>
  <div>Checked names: {{ checkedNames }}</div>

  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
  <label for="jack">Jack</label>

  <input type="checkbox" id="john" value="John" v-model="checkedNames" />
  <label for="john">John</label>

  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
  <label for="mike">Mike</label>
</template>
```

<FormInputCheckbox class="mt-5" />

---
transition: fade-out
---

# Form Input Checkbox

````md magic-move
```vue-html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no" />
```
```vue-html
<input
  type="checkbox"
  v-model="toggle"
  :true-value="dynamicTrueValue"
  :false-value="dynamicFalseValue" />
```
````

---
transition: fade-out
---

# Form Input Select

````md magic-move
```vue
<script setup>
  const selected = ref("A")
</script>

<template>
  <div>Selected: {{ selected }}</div>

  <select v-model="selected">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
</template>
```
```vue
<script setup>
  const selected = ref([])
</script>

<template>
  <div>Selected: {{ selected }}</div>

  <select multiple v-model="selected">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
</template>
```
```vue
<script setup>
  const selected = ref([])
</script>

<template>
  <div>Selected: {{ selected }}</div>

  <select multiple v-model="selected">
    <option :value="{ code: 'A' }">A</option>
    <option :value="{ code: 'B' }">B</option>
    <option :value="{ code: 'C' }">C</option>
  </select>
</template>
```
````

<FormInputSelect v-after class="mt-5" />

---
transition: fade-out
---

# Form Input Modifier

```vue
<!-- Nutzt @change anstatt von @input -->
<input v-model.lazy="msg" />
```

<v-clicks>

```vue
<!-- Casted mit parseFloat -->
<input v-model.number="age" />
```

```vue
<!-- Entfernt whitespace -->
<input v-model.trim="msg" />
```

</v-clicks>

<!-- 
@change -> Änderung und focus verloren
-->

---
transition: fade
---

# Side Effects

````md magic-move
```vue
<script setup>
const question = ref('')
const answer = ref('')
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    try {
      answer.value = (await (await fetch('https://yesno.wtf/api')).json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    } finally {
      loading.value = false
    }
  }
})
</script>

<template>
  <p>Stelle eine Ja oder Nein Frage: <input v-model="question" :disabled="loading" /></p>
  <p>{{ answer }}</p>
</template>
```
```vue
<script setup>
const question = ref('')
const answer = ref('')
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    try {
      answer.value = await (await fetch('https://yesno.wtf/api')).json().answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    } finally {
      loading.value = false
    }
  }
})
</script>
```
````

<SideEffects v-after class="mt-5" />

---
transition: fade-out
---

# Side Effects

````md magic-move
```js
const itemId = ref(1)
const data = ref(null)

watch(
  itemId,
  async () => {
    const response = await fetch(`https://server:port/items/${itemId.value}`)
    data.value = await response.json()
  }
)
```
```js
const itemId = ref(1)
const data = ref(null)

watch(
  itemId,
  async () => {
    const response = await fetch(`https://server:port/items/${itemId.value}`)
    data.value = await response.json()
  },
  { immediate: true }
)
```
```js
const itemId = ref(1)
const data = ref(null)

watchEffect(async () => {
  const response = await fetch(`https://server:port/items/${itemId.value}`)
  data.value = await response.json()
})
```
````

<!-- 
Beispiel Detailansicht
watchEffect tracked dependencies
-->

---
transition: fade-out
---

# Composables

````md magic-move
```vue
<script setup>
const question = ref('')
const answer = ref('')
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    try {
      answer.value = await (await fetch('https://yesno.wtf/api')).json().answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    } finally {
      loading.value = false
    }
  }
})
</script>
```
```js
// api.ts
export function useAskYesNoQuestion() {
  const question = ref('')
  const answer = ref('')
  const loading = ref(false)
  
  watch(question, async (newQuestion, oldQuestion) => {
    if (newQuestion.includes('?')) {
      loading.value = true
      answer.value = 'Thinking...'
      try {
        answer.value = await (await fetch('https://yesno.wtf/api')).json().answer
      } catch (error) {
        answer.value = 'Error! Could not reach the API. ' + error
      } finally {
        loading.value = false
      }
    }
  })

  return { question, answer, loading }
}
```
```vue
<script setup>
import { useAskYesNoQuestion } from './api.ts'

const { question, answer, loading } = useAskYesNoQuestion()
</script>
```
````

---
transition: fade
---

# Components

````md magic-move
```vue
<!-- ButtonCounter.vue -->
<script setup>
  import { ref } from 'vue'

  const count = ref(0)
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps } from 'vue'

  const count = ref(0)
  const props = defineProps<{ steps: number }>()
</script>

<template>
  <button @click="count++">Count is: {{ count * props.steps }}</button>
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps, withDefaults } from 'vue'

  const count = ref(0)
  const props = withDefaults(defineProps<{ steps: number }>(), {
    steps: 1
  })
</script>

<template>
  <button @click="count++">Count is: {{ count * props.steps }}</button>
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps } from 'vue'

  const count = ref(0)
  // Ab v3.5 mit Reactive Props Destructure
  const { steps = 1 } = defineProps<{ steps: number }>()
</script>

<template>
  <button @click="count++">Count is: {{ count * steps }}</button>
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps } from 'vue'

  const count = ref(0)
  const { steps = 1 } = defineProps<{ steps: number }>()
  const emit = defineEmits<{
    (e: 'reset'): void
  }>()
</script>

<template>
  <button @click="count++">Count is: {{ count * steps }}</button>
  <button @click="emit("reset")" />
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps } from 'vue'

  const count = ref(0)
  const { steps = 1 } = defineProps<{ steps: number }>()
  const emit = defineEmits<{
    (e: 'reset', newCount: number): void
  }>()
</script>

<template>
  <button @click="count++">Count is: {{ count * steps }}</button>
  <button @click="emit("reset", 10)" />
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps } from 'vue'

  const count = ref(0)
  const { steps = 1 } = defineProps<{ steps: number }>()
  const emit = defineEmits<{
    reset: [newCount: number] // Ab v3.3
  }>()
</script>

<template>
  <button @click="count++">Count is: {{ count * steps }}</button>
  <button @click="emit("reset", 10)" />
</template>
```
```vue
<!-- ButtonCounter.vue -->
<script setup lang="ts">
  import { ref, defineProps } from 'vue'

  const count = ref(0)
  const { steps = 1 } = defineProps<{ steps: number }>()
  const emit = defineEmits<{
    reset: [newCount: number]
  }>()
</script>

<template>
  <button v-bind="$attrs" @click="count++">Count is: {{ count * steps }}</button>
  <button @click="emit("reset", 10)" />
</template>
```
````

````md magic-move {at:7}
```vue
<!-- OtherComponent.vue -->
<script setup>
  import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <ButtonCounter />
</template>
```
```vue
<!-- OtherComponent.vue -->
<script setup>
  import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <ButtonCounter class="red-button" />
</template>
```
````

<!--
Jetzt haben wir alles um eine funktionale Anwendung zu bauen
Multiple root nodes
-->

---
transition: fade
---

# Component Slots

````md magic-move
```vue
<!-- Card.vue -->
<script setup lang="ts">
  const props = defineProps<{ title: string, body: string }>()
</script>

<template>
  <div>
    <h3>{{ props.title }}</h3>
    <p>{{ props.body }}</p>
  </div>
</template>
```
```vue
<!-- Card.vue -->
<script setup lang="ts">
  const props = defineProps<{ title: string }>()
</script>

<template>
  <div>
    <h3>{{ props.title }}</h3>
    <slot />
  </div>
</template>
```
```vue
<!-- Card.vue -->
<script setup lang="ts">
</script>

<template>
  <div>
    <slot name="title" />
    <slot>Keine Informationen</slot>
  </div>
</template>
```
````

````md magic-move {at:1}
```vue
<script setup>
  import Card from './Card.vue'
</script>

<template>
  <Card 
    title="Vue.js" 
    body="An approachable, performant and versatile framework for building web user interfaces."
  />
</template>
```
```vue
<script setup>
  import Card from './Card.vue'
</script>

<template>
  <Card title="Vue.js">
    An approachable, performant and versatile framework for building web user interfaces.
  </Card>
</template>
```
```vue
<script setup>
  import Card from './Card.vue'
</script>

<template>
  <Card title="Vue.js">
    <template v-slot:title>
      <h2>Vue.js</h2>
    </template>
    An approachable, performant and versatile framework for building web user interfaces.
  </Card>
</template>
```
```vue
<script setup>
  import Card from './Card.vue'
</script>

<template>
  <Card title="Vue.js">
    <template #title>
      <h2>Vue.js</h2>
    </template>
    An approachable, performant and versatile framework for building web user interfaces.
  </Card>
</template>
```
```` 

<!--
Limitiert - Was wenn es nicht h3 sondern h2 sein soll
slot - wie outlet von react router oder slot von web components
-->

---
transition: fade
---

# Component Slots

````md magic-move
```vue
<template>
  <div class="card">
    <div class="card-header">
      <slot name="title" />
    </div>
    <slot>Keine Informationen</slot>
  </div>
</template>
```
```vue
<template>
  <div class="card">
    <div v-if="$slots.header" class="card-header">
      <slot name="title" />
    </div>
    <slot>Keine Informationen</slot>
  </div>
</template>
```
````
<!-- 
Wenn Title nicht gesetzt ist soll div card header nicht auftauchen
-->

---
transition: fade-out
---

# Component Slots

````md magic-move
```vue
<!-- List.vue -->
<template>
  <ul>
    <li v-for="item in items">
      <slot name="item" />
    </li>
  </ul>
</template>
```
```vue
<!-- List.vue -->
<template>
  <ul>
    <li v-for="item in items">
      <slot name="item" :name="item.name" :rating="item.rating" />
    </li>
  </ul>
</template>
```
```vue
<!-- List.vue -->
<template>
  <ul>
    <li v-for="item in items">
      <slot name="item" v-bind="item" />
    </li>
  </ul>
</template>
```
````

````md magic-move
```vue
<!-- Main.vue -->
<template>
  <List>
    <template #item>
      Das ist nicht so sinnvoll
    </template>
  </List>
</template>
```
```vue
<!-- Main.vue -->
<template>
  <List>
    <template #item="{ name, rating }">
      {{ name }} - {{ rating }} Bewertung
    </template>
  </List>
</template>
```
````

---
transition: fade-out
---

# Dynamic Components

````md magic-move
```vue
<script setup>
import Home from './Home.vue'
import Posts from './Posts.vue'
import Archive from './Archive.vue'
import { ref } from 'vue'
 
const currentTab = ref('Home')
const tabs = ['Home', 'Posts', 'Archive']
</script>

<template>
  <button
    v-for="tab in tabs"
    :key="tab"
    @click="currentTab = tab"
   >
    {{ tab }}
  </button>
  <Home v-if="currentTab === 'Home'" />
  <Posts v-if="currentTab === 'Posts'" />
  <Archive v-if="currentTab === 'Archive'" />
</template>
```
```vue
<script setup>
// imports...
 
const currentTab = ref('Home')
const tabs = { Home, Posts, Archive }
</script>

<template>
  <button
    v-for="(_, tab) in tabs"
    :key="tab"
    @click="currentTab = tab"
   >
    {{ tab }}
  </button>
  <component :is="tabs[currentTab]" />
</template>
```
````

<DynamicComponents v-after class="mt-5" />


---
transition: fade-out
---

# Styling Block


````md magic-move
```vue
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```
```vue
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```
```vue
<style module>
.example {
  color: red;
}
</style>

<template>
  <div :class="$style.red">hi</div>
</template>
```
```vue
<style scoped>
div {
  /* Wird eine CSS Variable */
  color: v-bind('theme.color');
}
</style>

<template>
  <div>hi</div>
</template>

<script setup>
  import { ref } from 'vue'
  const theme = ref({
      color: 'red',
  })
</script>
```
```` 
---
transition: fade-out
---

# CSS Classes

````md magic-move
```vue
<template>
  <div :class="(isActive ? 'active' : '') + (hasError ? ' text-danger' : '') + ' static'" />
</template>
```
```vue
<template>
  <div 
    class="static"
    :class="{ 'active': isActive, 'text-danger': hasError }" />
</template>
```
```vue
<template>
  <div 
    class="static"
    :class="[activeClasses, errorClasses]" />
</template>
```
```vue
<template>
  <div 
    class="static"
    :class="[{ [activeClasses]: isActive }, errorClasses]" />
</template>
```
````

---
transition: fade-out
---

# Ecosystem and Tools

- Vite - Build Tool
- Vitest - Testing Framework
- Nuxt - Metaframework für SSR, SSG, ESG, ISR oder Hybrid
- Vueuse - Composition Utilities
- Tanstack Query - Asynchronous state management
- radix-vue - Unstyled, accessible Components

<!--
Präsentation schon zu lange
Jeder Punkt seine eigene Präsentation
Nuxt - Server Side Rendering, Static Site Generation, Edge Side Rendering, Incremental Static Regeneration
Diese Präsentation basiert größten Teils auf der Dokumentation
-->

---
transition: fade-out
---

# Die Zukunft von Vue: Vapor Mode

<div class="grid grid-cols-2 gap-4">
  <div>
    
```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <div>
    <button @click="count++">
      {{ count }}
    </button>
  </div>
</template>
```

  </div>
  <div>
    
```js
// Vereinfachter Code

const t0 = template("<div><button></button></div>")
delegateEvents("click")

export default {
  ...
  setup(props, { expose }) {
    expose();
    const count = ref(0)
    return { count, ref }
  }
  render(ctx) {
    const div = t0()
    const b = div.firstChild
    delegate(b, "click", () => () => (ctx.count++))
    renderEffect(() => setText(b, ctx.count))

    return div
  }
}
```

  </div>
</div>

<!-- 
Kein Virtual DOM
-->

---
transition: fade-out
---

# Ausblick

- KeepAlive
- Transition/-Group
- Teleport
- Suspense and Async Components
- Provide/Inject
- Template Refs
- Eigene Plugins oder Directives
- ...

<!-- 
Viel wieder erkennen von Dokumentation in den Folien
-->

---
transition: fade-out
layout: center
class: 'text-center pb-5 :'
---

# Danke!

Die Folien gibt es hier [github.com/mello-r/vue-introduction](https://github.com/mello-r/vue-introduction)