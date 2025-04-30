<template>
  <div class="app">
    <LoveTalk />
  </div>
</template>

<script setup>
import LoveTalk from './components/LoveTalk.vue'
</script>

<style>
body {
  margin: 0;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
  font-family: "Microsoft YaHei", sans-serif;
  padding: 20px;
}

.app {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style> 