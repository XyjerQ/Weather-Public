<script setup lang="ts">
import { Icon } from "@iconify/vue";

const props = defineProps({
  dt: { type: Number, required: true },
  temp: { type: Number, required: true },
  humidity: { type: Number, required: true },
  pressure: { type: Number, required: true },
  status: { type: String, required: true },
  statusIcon: { type: String, required: true },
  wind: { type: Number, required: true },
});

function timeConverter(UNIX_timestamp: number) {
  var a = new Date(UNIX_timestamp * 1000);
  var months = [
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December",
  ];
  var month = months[a.getMonth()];
  var day = a.getDate();
  var time = `${day} ${month}`;
  return time;
}
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
    <div id="date">{{ timeConverter(props.dt) }}</div>
    <div id="time">{{ getHoursMinutes(props.dt) }}</div>
    <div id="temp">{{ props.temp.toFixed(0) }}°C</div>
    <div id="statusIcon">
      <img
        :src="`https://openweathermap.org/img/wn/${statusIcon}.png`"
        :alt="props.status"
      />
    </div>
    <div id="status">{{ props.status }}</div>
    <div id="humidity">
      <Icon icon="mdi:drop" color="#0072b6" />{{ props.humidity }}%
    </div>
    <div id="pressure">{{ props.pressure }} hPa</div>
    <div id="wind">
      <Icon icon="tabler:wind" /> {{ props.wind.toFixed(2) }} m/s
    </div>
  </div>
</template>

<style lang="scss" scoped>
#forecast {
  margin-top: 1rem;
  display: grid;
  grid-template-columns: 18.5% 7% 10% 5% 8% 12% 15% 20% 20%;
  gap: 10px;
  text-align: center;
  padding: 5px;
  color: white;
  text-shadow: 3px 3px #0000005e;
  font-size: 2rem;
  background-color: rgba(255, 255, 255, 0.438);
  border-radius: 20px;
  box-shadow: 6px 6px #0000005e;
  width: 1200px;
  margin-inline: auto;
  svg {
    font-size: 40px;
  }
}
#status {
  text-align: left;
}
#statusIcon {
  text-align: right;
}
#date {
  text-align: left;
  padding-left: 10px;
}
svg {
  -webkit-filter: drop-shadow(3px 3px #0000005e);
  filter: drop-shadow(3px 3px #0000005e);
}
img {
  -webkit-filter: drop-shadow(3px 3px #0000005e);
  filter: drop-shadow(3px 3px #0000005e);
}
@media only screen and (max-width: 1240px) {
  #forecast {
    margin-top: 0.8rem;
    display: grid;
    grid-template-columns: 40% 10% 22% 8% 20%;
    gap: 2px;
    text-align: center;
    padding: 2px;
    color: white;
    text-shadow: 2px 3px #0000005e;
    font-size: 2.2rem;
    box-shadow: 5px 5px #0000005e;
    width: 95%;
  }
  img {
    -webkit-filter: drop-shadow(2px 2px #0000005e);
    filter: drop-shadow(2px 2px #0000005e);
    width: 55px;
  }
  #wind {
    display: none;
  }
  #pressure {
    display: none;
  }
  #humidity {
    display: none;
  }
}
@media only screen and (max-width: 670px) {
  #forecast {
    margin-top: 0.8rem;
    display: grid;
    grid-template-columns: 38% 12% 18% 10% 15%;
    gap: 2px;
    text-align: center;
    padding: 2px;
    color: white;
    text-shadow: 2px 3px #0000005e;
    font-size: 1.1rem;
    box-shadow: 5px 5px #0000005e;
    width: 95%;
  }
  img {
    -webkit-filter: drop-shadow(2px 2px #0000005e);
    filter: drop-shadow(2px 2px #0000005e);
    width: 32px;
  }
}
</style>
