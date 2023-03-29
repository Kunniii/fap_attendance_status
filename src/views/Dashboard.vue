<script setup>
import Card from "../components/Card.vue";
import axios from "axios";
import { ref } from "vue";
import RollingSVG from "../assets/rolling.svg";

let apiData = ref(null);
let isLoading = ref(true);
let selectedTerm = "";
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
  isLoading.value = true;
  makeRequest({ id: sid });
};

const getRequestParams = (t) => {
  if (apiData) {
    let { term, campus } = apiData.terms.terms[t].requestParams;
    return { term, campus };
  }
};

const requestNewData = () => {
  if (selectedTerm == apiData.terms.current) {
    return;
  }
  console.log(selectedTerm);
  let params = { id: sid, ...getRequestParams(selectedTerm) };
  console.log(params);
  makeRequest(params);
};

const makeRequest = (params) => {
  localStorage.removeItem("savedData");
  isLoading.value = true;
  axios
    .post(
      "https://fapapi.cyclic.app/attendance",
      { ...params },
      {
        headers: {
          accept: "application/json",
          "content-type": "application/json",
        },
      }
    )
    .then((res) => {
      window.location.reload();
      apiData.value = res.data.data;
      localStorage.setItem("savedData", JSON.stringify({ data: res.data.data, time: Date.now() }));
      isLoading.value = false;
      selectedTerm = res.data.data.terms.current;
    })
    .catch((err) => {
      alert(err.message);
    });
};

if (savedData && savedData.data) {
  if (Date.now() - savedData.time > revalidateTime) {
    makeRequest({ id: sid });
  } else {
    apiData = savedData.data;
    isLoading.value = false;
    selectedTerm = savedData.data.terms.current;
  }
} else {
  makeRequest({ id: sid });
}
</script>

<template>
  <button
    class="fixed text-sm top-3 left-3 border-4 px-1 border-red-500 font-bold text-white rounded-lg"
    @click="logout"
    title="Logout"
  >
    ❌
  </button>
  <button
    class="fixed top-3 right-3 border-4 px-2 border-cyan-500 text-cyan-500 rounded-lg uppercase"
    @click="forceReload"
    title="Force Reload to get new data"
  >
    reload
  </button>
  <div
    v-if="isLoading"
    class="flex"
  >
    <div class="mx-auto pt-32">
      <img
        :src="RollingSVG"
        class="animate-spin mx-auto my-auto text-xl w-36"
      />
    </div>
  </div>

  <div
    v-else
    class="w-1/2 md:w-3/4 lg:w-1/2 mx-auto"
  >
    <h1 class="text-center font-bold pt-24 pb-10 text-3xl">
      {{ apiData?.terms.current }}
    </h1>

    <div>
      <label for="term">Select Term: </label>
      <select
        name="term"
        id="term"
        title="Select term to see result"
        @change="requestNewData"
        v-model="selectedTerm"
      >
        <option
          v-for="term of Object.keys(apiData.terms.terms)"
          :value="term"
        >
          {{ term }}
        </option>
      </select>
    </div>

    <div
      v-if="!isLoading"
      :key="apiData.terms.current"
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-10 pt-10"
    >
      <Card
        v-for="course in apiData.reports"
        :key="course.name"
        :course="course"
      />
    </div>
  </div>
</template>
