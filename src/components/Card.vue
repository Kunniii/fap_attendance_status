<script setup>
const props = defineProps({
  course: Object,
});

let report;

function analyzeReport(report) {
  let total = report.length;
  let availablePercent = 0.2;
  let availableAbsents = Math.floor(availablePercent * total);

  let absents = 0;
  for (let slot of report) {
    if (slot["Attendance status"] == "Absent") {
      absents++;
    }
  }
  let absentPercent = Math.ceil((absents / total) * 100);
  return { absentPercent, availableAbsents, absents };
}

function colorBasedOnPercent(percent) {
  if (percent <= 5) {
    return "bg-green-500 hover:scale-110 text-white border-green-600";
  }
  if (percent <= 10) {
    return "bg-yellow-500 hover:scale-110 text-white border-yellow-600";
  }
  if (percent <= 15) {
    return "bg-orange-500 hover:scale-110 text-white border-orange-600";
  }
  if (percent < 20) {
    return "bg-red-500 hover:scale-110 text-white border-red-600";
  }
  return "bg-black hover:scale-110 text-white";
}

report = analyzeReport(props.course.reports);
</script>

<template>
  <div
    class="border-2 py-9 px-3 duration-200 ease-in-out font-bold rounded-xl"
    :class="colorBasedOnPercent(report.absentPercent)"
    :title="
      'You have ' +
      report.availableAbsents +
      ' absent slots. You\'ve absent for ' +
      report.absents +
      ' slot(s) so far!'
    "
  >
    <h1>{{ course.name }}</h1>
    <hr class="my-4" />
    <div class="flex justify-around">
      <p>{{ report?.absentPercent }} %</p>
      <p>{{ report?.absents }} / {{ report?.availableAbsents }}</p>
    </div>
  </div>
</template>
