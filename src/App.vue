<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import CalorieCalculator from './components/CalorieCalculator.vue';
import { darkTheme, enGB, dateEnGB } from 'naive-ui';
</script>

<template>
  <n-config-provider :theme="darkTheme" :locale="enGB" :date-locale="dateEnGB">
    <CalorieCalculator />
    <n-global-style />
  </n-config-provider>
</template>

<style>
/* body {
  background-color: rgb(16, 16, 20);
} */
</style>
