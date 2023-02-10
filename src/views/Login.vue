<script setup>
import { ref } from "vue";

let sid = ref("");
const handleLoginClick = () => {
  console.log(sid.value);
};
</script>

<template>
  <div class="flex flex-col justify-center">
    <h1>Welcome to FAP Attendance Status</h1>

    <input
      class="text-center border"
      type="text"
      placeholder="Your SessionID..."
      v-model="sid"
    />
    <button @click="handleLoginClick">Continue</button>
  </div>
</template>
