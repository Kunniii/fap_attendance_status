<script setup>
import Dashboard from "./views/dashboard.vue";
import Login from "./views/login.vue";

const sid = localStorage.getItem("sid");
</script>

<template>
  <div class="w-96 mx-auto">
    <Login />
  </div>
</template>
