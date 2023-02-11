<script setup>
import { ref } from "vue";

let sid = ref("");
const handleLoginClick = () => {
  console.log(sid.value);
  if (!sid.value) {
    return;
  }
  localStorage.setItem("sid", sid.value);
};
</script>

<template>
  <div class="w-96 mx-auto">
    <h1 class="font-bold text-center my-20 text-4xl">
      Welcome to FAP Attendance Status
    </h1>
    <form class="mt-40 flex flex-col justify-center" autocomplete="on">
      <input
        class="text-center border rounded focus:border-2 w-96 mx-auto py-2 mb-10 focus:outline-none"
        type="text"
        placeholder="Your SessionID..."
        v-model="sid"
        autocomplete="on"
      />
      <button
        class="bg-sky-500 font-bold text-white w-fit px-4 py-2 rounded-md mx-auto"
        @click="handleLoginClick"
      >
        Continue
      </button>
    </form>

    <footer class="my-10">
      
    <div class="mt-96 border p-4 rounded">
      <p class="text-justify">
        This website is used for checking Attendance Status instead of checking
        it on FAP.
      </p>
      <p>
        You will need to provide a SessionID in order for the website to make
        request and retrive data from FAP.
      </p>
      <p>
        You can find your SessionID after you haved loggin on FAP by open Chrome
        Developer Tools (press F12), go to Application tab, choose Cookies and
        copy value of <pre class="inline">ASP.NET_SessionId</pre>. Then paste it in this website.
      </p>
    </div>
    </footer>
</div>
</template>
