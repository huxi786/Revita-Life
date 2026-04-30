<script setup>
import Navbar from './components/Navbar.vue';
import ContactAndFooter from './components/ContactAndFooter.vue';
</script>

<template>
  <div class="app">
    <Navbar />
    <router-view />
    <ContactAndFooter />
  </div>
</template>

<style>
/* App global styles if needed */
</style>
