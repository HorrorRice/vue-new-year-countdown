<script setup>
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";
import {reactive, watchEffect} from "vue";

// Reactive countdown values as a single object
const countdown = reactive({
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0,
});

function calculateCountdown() {
  const now = new Date();
  const newYear = new Date(now.getFullYear() + 1, 0, 1);

  // Calculate the difference in milliseconds
  const diffInMs = newYear - now;

  // Calculate days directly
  countdown.days = Math.floor(diffInMs / (1000 * 60 * 60 * 24));

  // Calculate remaining time for hours, minutes, and seconds
  countdown.hours = 23 - now.getHours();
  countdown.minutes = 59 - now.getMinutes();
  countdown.seconds = 59 - now.getSeconds();
}

// Watch for updates and recalculate the countdown
watchEffect(() => {
  const timer = setInterval(calculateCountdown, 1000); // Re-calculate remaining time every second just to keep the timer running/.

  // Clear interval.
  return () => clearInterval(timer);
});
</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader/>
      <main class="flex justify-center">
          <CountdownSegment data-test="days" label="days" :number="countdown.days"/>
          <CountdownSegment data-test="hours" label="hours" :number="countdown.hours"/>
          <CountdownSegment data-test="minutes" label="minutes" :number="countdown.minutes"/>
          <CountdownSegment data-test="seconds" label="seconds" :number="countdown.seconds"/>
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}

.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}

body {
  @apply bg-gray-100;
}
</style>
