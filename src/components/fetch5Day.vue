<script lang="ts" setup>
import { ref, reactive, onMounted } from "vue";
import * as apikey from "@/Api/apikey";
import Display5Day from "./display5Day.vue";
import DisplayDay from "./displayDay.vue";

const props = defineProps({
  mainCords: { type: Object },
});

const lat = ref("");
const lon = ref("");
const units = ref("metric");
const forecastType = ref("Day");

interface forecastObject {
  dt: number;
  dt_txt: string;
  main: any;
  weather: any;
  wind: any;
}
const fetchForecast = reactive<Array<forecastObject>>([]);
const fetchForecastDay = reactive<Array<forecastObject>>([]);

const fetch5Day = () => {
  console.log(props.mainCords);
  if (props.mainCords != undefined) {
    lat.value = props.mainCords[0].lat;
    lon.value = props.mainCords[0].lon;
  }
  const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${lat.value}&lon=${lon.value}&units=${units.value}&appid=${apikey.key}`;
  fetch(url)
    .then((response) => response.json())
    .then((response) => {
      console.log(response.list);
      fetchForecast.length = 0;
      fetchForecastDay.length = 0;
      for (let i = 0; i < response.list.length; i++) {
        fetchForecast.push(response.list[i]);
      }
      for (let i = 0; i <= 7; i++) {
        fetchForecastDay.push(response.list[i]);
      }
      console.log(fetchForecast);
    });
};

onMounted(() => {
  fetch5Day();
});

defineExpose({
  fetch5Day,
});
</script>
<template>
  <div id="castApp">
    <h1>Forecast:</h1>
    <form class="form">
      <div class="switch-field">
        <input
          v-model="forecastType"
          type="radio"
          id="radio-one"
          name="switch-one"
          value="Day"
          checked
        />
        <label for="radio-one">Day</label>
        <input
          v-model="forecastType"
          type="radio"
          id="radio-two"
          name="switch-one"
          value="5Days"
        />
        <label for="radio-two">5 Days</label>
      </div>
    </form>
    <div
      id="dispForecast"
      v-if="forecastType == 'Day'"
      style="display: flex; justify-content: center"
    >
      <DisplayDay
        v-for="forecast in fetchForecastDay"
        :key="forecast.dt"
        :dt="forecast.dt"
        :temp="forecast.main.temp"
        :humidity="forecast.main.humidity"
        :status="forecast.weather[0].main"
        :statusIcon="forecast.weather[0].icon"
      />
    </div>
    <div id="dispForecast" v-else-if="forecastType == '5Days'">
      <Display5Day
        v-for="forecast in fetchForecast"
        :key="forecast.dt_txt"
        :dt="forecast.dt"
        :temp="forecast.main.temp"
        :humidity="forecast.main.humidity"
        :pressure="forecast.main.pressure"
        :status="forecast.weather[0].main"
        :statusIcon="forecast.weather[0].icon"
        :wind="forecast.wind.speed"
      />
    </div>
  </div>
</template>

<style lang="scss">
#castApp {
  display: block;
  text-align: center;
  color: white;
  text-shadow: 5px 5px #0000005e;
  h1 {
    font-size: 6rem;
    margin-bottom: 0px;
  }
}
.switch-field {
  display: flex;
  overflow: hidden;
  background-color: rgba(255, 255, 255, 0.438);
  border-radius: 20px;
  box-shadow: 6px 6px #0000005e;
  margin-bottom: 1rem;
}

.switch-field input {
  position: absolute !important;
  clip: rect(0, 0, 0, 0);
  border: 0;
  overflow: hidden;
}

.switch-field label {
  width: 300px;
  color: white;
  font-size: 4rem;
  text-align: center;
}

.switch-field label:hover {
  cursor: pointer;
}

.switch-field input:checked + label {
  background-color: rgba(255, 255, 255, 0.38);
  box-shadow: none;
}
.form {
  display: flex;
  justify-content: center;
}
@media only screen and (max-width: 670px) {
  #castApp {
    text-shadow: 4px 4px #0000005e;
    h1 {
      font-size: 3rem;
    }
  }
  .switch-field {
    border-radius: 20px;
    box-shadow: 6px 6px #0000005e;
    margin-bottom: 1rem;
  }

  .switch-field label {
    width: 210px;
    font-size: 2.2rem;
    text-shadow: 3px 3px #0000005e;
  }
}
@media only screen and (max-width: 450px) {
  #castApp {
    text-shadow: 4px 4px #0000005e;
    h1 {
      font-size: 2.4rem;
    }
  }
  .switch-field {
    border-radius: 20px;
    box-shadow: 6px 6px #0000005e;
    margin-bottom: 1rem;
  }

  .switch-field label {
    width: 167px;
    font-size: 1.8rem;
    text-shadow: 3px 3px #0000005e;
  }
}
</style>
