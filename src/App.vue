<script setup>
import Dashboard from "./views/Dashboard.vue";
import Login from "./views/Login.vue";

const sid = localStorage.getItem("sid");
</script>

<template>
  <div class="mx-auto">
    <Login v-if="!sid" />
    <Dashboard v-else />
  </div>
</template>
