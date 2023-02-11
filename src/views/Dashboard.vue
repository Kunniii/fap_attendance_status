<script setup>
import Card from "../components/Card.vue";
import axios from "axios";
import { ref } from "vue";

let apiData = ref(null);
let sid = localStorage.getItem("sid");

const logout = () => {
  localStorage.removeItem("sid");
  window.location.reload();
};

axios
  .post(
    "https://fapapi.cyclic.app/attendance",
    { id: sid },
    {
      headers: {
        accept: "application/json",
        "content-type": "application/json",
      },
    }
  )
  .then((res) => (apiData.value = res.data.data));
</script>

<template>
  <div class="w-96 mx-auto">
    <button
      class="fixed text-sm top-1 left-1 border-4 border-red-500 font-bold text-white rounded-lg"
      @click="logout"
    >
      ❌
    </button>

    <h1 class="text-center font-bold pt-24 text-3xl">
      {{ apiData?.terms.current }}
    </h1>
    <div class="grid grid-cols-1 pt-20">
      <Card
        v-for="course in apiData?.reports"
        :key="course.name"
        :course="course"
      />
    </div>
  </div>
</template>
