---
theme: apple-basic
title: Vue.js - Nutzung, Unterschiede und sein Ecosystem
lineNumbers: true

layout: intro
transition: fade-out
---

# Vue.js

Einblick in das JavaScript Framework

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/mello-r/vue-introduction" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

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
const obj = reactive({ count: 0 })
console.log(obj.value) // 0

obj.count++

count.value++
console.log(count.value) // 1
```

<v-click>

Limitationen

```js
const state = reactive({ count: 0 })

// reactivity verloren
state = { count: 1 }

// reactivity verloren
let { count } = state
count++

// reactivity verloren
callSomeFunction(state.count)
```
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


---
transition: fade-out
---

# Class and Style Bindings


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
<div v-bind="objectOfAttrs"></div>
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
<p v-show="seen">Ich werde angezeigt wenn 'seen' truthy ist</p>
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

# Input Bindings

https://vuejs.org/guide/essentials/forms.html

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
<script setup>
  const { t } = useI18n({ ... })
</script>

<template>
<p>{{ t('hello') }}</p>
</template>

<style>
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
transition: fade-out
---

# Styling

- https://vuejs.org/api/sfc-css-features.html
- scoped - Wie sieht es nach dem render aus
- mehrere style tags
- module (Module name geben)
- verschiedene sprachen beispielsweise Scss
- v-bind in style

---
transition: fade-out
---

# Unterschiede

- Options API
- Single File Components
- lifecyle hooks
- custome directivs (v-toggle)
- plugin system
  - beispielsweise custom-blocks in sfc
- Einzigen der großen drei ohne großen Konzern Angular = Google, React = Facebook
- Out of the box custom element (Web components) support 

---
transition: fade-out
---

# Ecosystem

- Dokumentation ist sehr gut

<!-- 
Diese Präsentation basiert größten Teils auf der Dokumentation
-->

---
transition: fade-out
---

# Gedanken noch nutzen
