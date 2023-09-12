<script setup lang="ts">
import { Icon } from "@iconify/vue";

const props = defineProps({
  dt: { type: Number, required: true },
  temp: { type: Number, required: true },
  humidity: { type: Number, required: true },
  status: { type: String, required: true },
  statusIcon: { type: String, required: true },
});

function getHoursMinutes(UNIX_timestamp: number) {
  var a = new Date(UNIX_timestamp * 1000);
  var hours = a.getHours();
  var minutes = a.getMinutes();
  var time = `${hours}:0${minutes}`;
  return time;
}
</script>

<template>
  <div id="forecast">
    <div id="time">{{ getHoursMinutes(props.dt) }}</div>
    <div id="statusIcon">
      <img
        :src="`https://openweathermap.org/img/wn/${statusIcon}.png`"
        :alt="props.status"
      />
    </div>
    <div id="temp">{{ props.temp.toFixed(0) }}°C</div>
    <div id="humidity">
      <Icon icon="mdi:drop" color="#0072b6" />{{ props.humidity }}%
    </div>
  </div>
</template>

<style lang="scss" scoped>
#forecast {
  width: 120px;
  float: left;
  background-color: rgba(255, 255, 255, 0.438);
  border-radius: 20px;
  box-shadow: 6px 6px #0000005e;
  font-size: 2rem;
  text-shadow: 3px 3px #0000005e;
  margin: 0.5rem;
}
#time {
  font-size: 1.8rem;
}
#temp {
  font-size: 2.3rem;
  height: 45px;
}
#humidity {
  font-size: 1.5rem;
  padding-bottom: 0.2rem;
}
img {
  width: 90px;
}
@media only screen and (max-width: 870px) {
  #forecast {
    width: 100px;
    box-shadow: 3px 3px #0000005e;
    font-size: 1.5rem;
    text-shadow: 2px 2px #0000005e;
    margin: 0.2rem;
  }
  #time {
    font-size: 1.5rem;
  }
  #temp {
    font-size: 1.8rem;
    height: 35px;
  }
  #humidity {
    font-size: 1.2rem;
    padding-bottom: 0.2rem;
  }
  img {
    width: 60px;
  }
}

@media only screen and (max-width: 670px) {
  #forecast {
    width: 55px;
    box-shadow: 3px 3px #0000005e;
    font-size: 1rem;
    text-shadow: 2px 2px #0000005e;
    margin: 0.2rem;
  }
  #time {
    font-size: 1rem;
  }
  #temp {
    font-size: 1.2rem;
    height: 25px;
  }
  #humidity {
    font-size: 0.8rem;
    padding-bottom: 0.2rem;
  }
  img {
    width: 40px;
  }
}
@media only screen and (max-width: 400px) {
  #forecast {
    width: 38px;
    box-shadow: 3px 3px #0000005e;
    font-size: 0.6rem;
    text-shadow: 2px 2px #0000005e;
    margin: 0.15rem;
    border-radius: 6px;
  }
  #time {
    font-size: 0.7rem;
  }
  #temp {
    font-size: 0.85rem;
    height: 20px;
  }
  #humidity {
    font-size: 0.6rem;
    padding-bottom: 0.2rem;
  }
  img {
    width: 25px;
  }
}
</style>
