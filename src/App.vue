<script setup lang="ts">
import Users from './components/Users.vue'
</script>

<template>
  <main>
    <h1>Список користувачів</h1>
    <Users />
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100%;
}

body {
  background: #f3f4f6;
  font-family: Arial, sans-serif;
}

main {
  width: 100%;
  min-height: 100vh;
  padding: 30px;
}

h1 {
  margin: 0 0 30px;
  text-align: center;
  color: #1f2937;
}
</style>