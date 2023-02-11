<script setup>
const props = defineProps({
  course: Object,
});

let report;

function analyzeReport(report) {
  let total = report.length;
  let avaiablePercent = 0.2;
  let availableAbsents = Math.floor(avaiablePercent * total);

  let absents = 0;
  for (let slot of report) {
    if (slot["Attedance status"] == "Absent") {
      absents++;
    }
  }
  let absentPercent = Math.round((absents / total) * 100);
  return { absentPercent, availableAbsents, absents };
}

function colorBasedOnPercent(percent) {
  if (percent <= 5) {
    return "hover:bg-green-500  hover:text-white hover:border-green-600 border-green-500";
  }
  if (percent <= 10) {
    return "hover:bg-yellow-500  hover:text-white hover:border-yellow-600 border-yellow-500";
  }
  if (percent <= 15) {
    return "hover:bg-orange-500  hover:text-white hover:border-orange-600 border-orange-500";
  }
  if (percent <= 20) {
    return "hover:bg-red-500  hover:text-white hover:border-red-600 border-red-500";
  }
  return "hover:bg-black  hover:text-white border-black";
}

report = analyzeReport(props.course.reports);
</script>

<template>
  <div
    class="border-2 my-10 py-5 px-3 duration-200 ease-in-out"
    :class="colorBasedOnPercent(report.absentPercent)"
  >
    <h1>{{ course.name }}</h1>
    <hr class="my-4" />
    <div class="flex justify-around">
      <p>{{ report?.absentPercent }} %</p>
      <p>{{ report?.absents }} / {{ report?.availableAbsents }}</p>
    </div>
  </div>
</template>
