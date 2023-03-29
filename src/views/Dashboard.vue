<script setup>
import Card from "../components/Card.vue";
import axios from "axios";
import { ref } from "vue";

let apiData = ref(null);
let sid = localStorage.getItem("sid");
let savedData = JSON.parse(localStorage.getItem("savedData"));
const revalidateTime = 3600000; //ms (1 hour)

const logout = () => {
  localStorage.removeItem("sid");
  localStorage.removeItem("data");
  window.location.reload();
};

const forceReload = () => {
  localStorage.removeItem("savedData");
  window.location.reload();
};

const makeRequest = () => {
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
    .then((res) => {
      apiData.value = res.data.data;
      localStorage.setItem("savedData", JSON.stringify({ data: res.data.data, time: Date.now() }));
    });
};

if (savedData && savedData.data) {
  if (Date.now() - savedData.time > revalidateTime) {
    makeRequest();
  } else {
    apiData = savedData.data;
  }
} else {
  makeRequest();
}
</script>

<template>
  <div class="w-1/2 md:w-3/4 lg:w-1/2 mx-auto">
    <button
      class="fixed text-sm top-1 left-1 border-4 border-red-500 font-bold text-white rounded-lg"
      @click="logout"
    >
      ❌
    </button>
    <button
      class="fixed text-sm top-1 right-1 border-4 border-cyan-500 font-bold bg-cyan-500 text-white rounded-lg uppercase"
      @click="forceReload"
    >
      force reload
    </button>

    <h1 class="text-center font-bold pt-24 text-3xl">
      {{ apiData?.terms.current }}
    </h1>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-10 pt-20">
      <Card
        v-for="course in apiData?.reports"
        :key="course.name"
        :course="course"
      />
    </div>
  </div>
</template>
