<script setup lang="ts">
import { ref, reactive } from "vue";
import fetchWeather from "@/components/fetchWeather.vue";
import fetch5Day from "@/components/fetch5Day.vue";
import SearchTown from "@/components/searchTown.vue";

interface cordsObject {
  lat: any;
  lon: any;
}
const mainCords = reactive<Array<cordsObject>>([
  { lat: "51.50", lon: "-0.13" },
]);
const weather = ref<any>(null);
const day = ref<any>(null);

const timeCheck = (time: any) => {
  let currentDate = Math.round(Date.now() / 1000);
  if (time.sunrise < currentDate && currentDate < time.sunset) {
    document.body.style.backgroundImage = "url(day.png)";
  } else {
    document.body.style.backgroundImage = "url(night.png)";
  }
};

const cords = (passedCords: cordsObject) => {
  mainCords.length = 0;
  mainCords.push(passedCords);
  weather.value.fetchWeather();
  day.value.fetch5Day();
};
</script>

<template>
  <main>
    <SearchTown @cords="cords" />
    <fetchWeather @date="timeCheck" :mainCords="mainCords" ref="weather" />
    <fetch5Day :mainCords="mainCords" ref="day" />
  </main>
</template>

<style lang="scss">
body {
  height: 100vh;
  background: url(day.png) no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
</style>
