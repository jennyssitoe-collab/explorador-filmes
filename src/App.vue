<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <header style="background: #d63384; color: white; padding: 30px 20px; text-align: center;">
    <h1 style="margin: 0; font-size: 36px;">Busque um filme</h1>
  </header>
  
  <main style="padding: 20px;">
    <RouterView />
  </main>
</template>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #fdf2f8; 
}
</style>