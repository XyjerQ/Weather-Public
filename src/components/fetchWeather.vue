<script setup lang="ts">
import { onMounted, ref } from "vue";
import { Icon } from "@iconify/vue";
import * as apikey from "@/Api/apikey";

const props = defineProps({
  mainCords: { type: Object },
});

const emit = defineEmits(["date"]);

const lat = ref("");
const lon = ref("");
const units = ref("metric");
const townName = ref("");
const date = ref("");
const temp = ref("");
const status = ref("");
const statusIcon = ref("");
const wind = ref("");
const pressure = ref("");
const humidity = ref("");

type sunType = {
  sunrise?: any;
  sunset?: any;
};
const sunTime = ref<sunType>({});

const sunRise = ref("");
const sunSet = ref("");

const fetchWeather = () => {
  console.log(props.mainCords);
  if (props.mainCords != undefined) {
    lat.value = props.mainCords[0].lat;
    lon.value = props.mainCords[0].lon;
  }
  const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat.value}&lon=${lon.value}&units=${units.value}&appid=${apikey.key}`;
  fetch(url)
    .then((response) => response.json())
    .then((response) => {
      console.log(response);
      townName.value = response.name + ", " + response.sys.country;
      date.value = timeConverter(response.dt);
      temp.value = response.main.temp.toFixed(0);
      status.value = response.weather[0].main;
      statusIcon.value = response.weather[0].icon;
      humidity.value = response.main.humidity;
      pressure.value = response.main.pressure;
      wind.value = response.wind.speed;
      sunTime.value = {
        sunrise: response.sys.sunrise,
        sunset: response.sys.sunset,
      };
      emit("date", sunTime.value);
      var rise = new Date(sunTime.value.sunrise * 1000);
      var set = new Date(sunTime.value.sunset * 1000);

      if (rise.getHours() < 10 && rise.getMinutes() < 10) {
        sunRise.value = `0${rise.getHours()}:0${rise.getMinutes()}`;
      } else if (rise.getHours() < 10) {
        sunRise.value = `0${rise.getHours()}:${rise.getMinutes()}`;
      } else if (rise.getMinutes() < 10) {
        sunRise.value = `0${rise.getHours()}:0${rise.getMinutes()}`;
      } else {
        sunRise.value = `${rise.getHours()}:${rise.getMinutes()}`;
      }
      if (set.getHours() < 10 && set.getMinutes() < 10) {
        sunSet.value = `0${set.getHours()}:0${set.getMinutes()}`;
      } else if (set.getHours() < 10) {
        sunSet.value = `0${set.getHours()}:${set.getMinutes()}`;
      } else if (set.getMinutes() < 10) {
        sunSet.value = `0${set.getHours()}:0${set.getMinutes()}`;
      } else {
        sunSet.value = `${set.getHours()}:${set.getMinutes()}`;
      }
    });
};

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
  var days = [
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
    "Sunday",
  ];
  var year = a.getFullYear();
  var month = months[a.getMonth()];
  var date = a.getDate();
  var day = days[a.getDay()];
  var time = day + ", " + date + " " + month + " " + year;
  return time;
}
onMounted(() => {
  fetchWeather();
});

defineExpose({
  fetchWeather,
});
</script>

<template>
  <div id="weatherApp">
    <div id="townName">
      <h1>{{ townName }}</h1>
    </div>
    <div id="date">
      <h5>{{ date }}</h5>
    </div>
    <div id="temp">
      <div id="displayTemp">
        <p>{{ temp }}°C</p>
      </div>
    </div>

    <div id="status">
      <div id="statusDisp">
        <span>
          <img
            :src="`https://openweathermap.org/img/wn/${statusIcon}@2x.png`"
            :alt="status"
          />
        </span>
        <span>{{ status }}</span>
      </div>
      <div id="sun">
        <div id="sunrise">
          <p><Icon icon="tabler:sunset-2" color="#ea833a" />{{ sunRise }}</p>
        </div>
        <div id="sunset">
          <p><Icon icon="tabler:sunset-2" color="#f6f031" />{{ sunSet }}</p>
        </div>
      </div>
    </div>
    <div id="statusWeather">
      <div id="humidity">
        <Icon icon="mdi:drop" color="#0072b6" />{{ humidity }}%
      </div>
      <div id="pressure">{{ pressure }} hPa</div>
      <div id="wind"><Icon icon="tabler:wind" /> {{ wind }} m/s</div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
#weatherApp {
  display: block;
  text-align: center;
  color: white;
  text-shadow: 5px 5px #0000005e;
  padding-top: 1.5rem;

  #townName {
    h1 {
      font-size: 5rem;
      margin-top: 20px;
      margin-bottom: 0px;
      line-height: 0.8;
    }
  }
  #date {
    h5 {
      font-weight: 400;
      font-size: 3rem;
    }
  }
  #temp {
    display: flex;
    justify-content: center;
    padding-top: 2rem;
  }
  #displayTemp {
    height: 300px;
    width: 600px;
    background-color: rgba(255, 255, 255, 0.438);
    border-radius: 20px;
    box-shadow: 8px 8px #0000005e;

    p {
      position: relative;
      left: 50%;
      top: 50%;
      -webkit-transform: translate(-50%, -50%);
      transform: translate(-50%, -50%);
      color: rgb(255, 255, 255);
      font-size: 16rem;
      font-weight: 800;
      text-shadow: 5px 15px #0000005e;
    }
  }
  #status {
    padding-top: 1rem;
    padding-bottom: 0.8rem;
    font-size: 4.5rem;
    font-weight: 800;
    text-shadow: 7px 7px #0000005e;
    display: flex;
    justify-content: center;
    #statusDisp {
      width: 384px;
      padding: 0.6rem;
      padding-left: 0px;
      background-color: rgba(255, 255, 255, 0.438);
      border-radius: 20px;
      box-shadow: 5px 5px #0000005e;
      img {
        width: 130px;
        -webkit-filter: drop-shadow(6px 6px #0000005e);
        filter: drop-shadow(6px 6px #0000005e);
      }
    }
  }
  #sun {
    text-align: center;
    #sunrise,
    #sunset {
      width: 50%;
      margin-left: 1rem;
    }
    p {
      width: 200px;
      color: rgb(255, 255, 255);
      font-size: 3rem;
      font-weight: 800;
      text-shadow: 5px 5px #0000005e;
      background-color: rgba(255, 255, 255, 0.438);
      border-radius: 20px;
      box-shadow: 5px 5px #0000005e;
      margin-bottom: 0px;
    }
    #sunrise {
      p {
        margin-bottom: 10px;
      }
    }
  }
  #wind {
    text-align: left;
  }
  svg {
    -webkit-filter: drop-shadow(3px 3px #0000005e);
    filter: drop-shadow(3px 3px #0000005e);
  }
  #statusWeather {
    text-align: center;
    font-weight: 500;
    width: 600px;
    margin: auto;
    margin-bottom: 0.6rem;
    text-shadow: 4px 4px #0000005e;
    font-size: 2.5rem;
    display: grid;
    grid-template-columns: 24% 32% 40%;
    gap: 2%;
    #wind,
    #humidity,
    #pressure {
      text-align: center;
      background-color: rgba(255, 255, 255, 0.438);
      border-radius: 20px;
      box-shadow: 5px 5px #0000005e;
    }
  }
}
@media only screen and (max-width: 670px) {
  #weatherApp {
    text-shadow: 4px 4px #0000005e;
    padding-top: 1rem;

    #townName {
      h1 {
        font-size: 3rem;
        margin-top: 20px;
        margin-bottom: 0px;
        line-height: 0.8;
      }
    }
    #date {
      h5 {
        font-weight: 400;
        font-size: 1.8rem;
      }
    }
    #temp {
      padding-top: 2rem;
    }
    #displayTemp {
      height: 180px;
      width: 420px;
      border-radius: 20px;
      box-shadow: 5px 5px #0000005e;

      p {
        position: relative;
        font-size: 9rem;
        font-weight: 800;
        text-shadow: 5px 6px #0000005e;
      }
    }
    #status {
      padding-top: 1rem;
      padding-bottom: 0.8rem;
      font-size: 2.8rem;
      text-shadow: 5px 5px #0000005e;
      #statusDisp {
        width: 274px;
        border-radius: 20px;
        box-shadow: 5px 5px #0000005e;
        img {
          width: 55px;
          -webkit-filter: drop-shadow(6px 6px #0000005e);
          filter: drop-shadow(6px 6px #0000005e);
        }
      }
    }
    #sun {
      text-align: center;
      #sunrise,
      #sunset {
        width: 50%;
        margin-left: 1rem;
      }
      p {
        width: 130px;
        font-size: 1.6rem;
        text-shadow: 3px 3px #0000005e;
        box-shadow: 5px 5px #0000005e;
      }
    }
    #statusWeather {
      max-width: 420px;
      margin: auto;
      margin-bottom: 0.6rem;
      text-shadow: 3px 3px #0000005e;
      font-size: 1.5rem;
      grid-template-columns: 24% 32% 40%;
      #wind,
      #humidity,
      #pressure {
        box-shadow: 5px 5px #0000005e;
      }
    }
  }
}
@media only screen and (max-width: 450px) {
  #weatherApp {
    text-shadow: 4px 4px #0000005e;
    padding-top: 0.8rem;

    #townName {
      h1 {
        font-size: 2.4rem;
        margin-top: 20px;
        margin-bottom: 0px;
        line-height: 0.8;
      }
    }
    #date {
      h5 {
        font-weight: 400;
        font-size: 1.3rem;
      }
    }
    #temp {
      padding-top: 1rem;
    }
    #displayTemp {
      height: 160px;
      width: 330px;
      border-radius: 20px;
      box-shadow: 5px 5px #0000005e;

      p {
        position: relative;
        font-size: 7rem;
        font-weight: 800;
        text-shadow: 5px 6px #0000005e;
      }
    }
    #status {
      padding-top: 1rem;
      padding-bottom: 0.8rem;
      font-size: 2.6rem;
      text-shadow: 4px 4px #0000005e;
      #statusDisp {
        width: 214px;
        border-radius: 20px;
        box-shadow: 5px 5px #0000005e;
        img {
          width: 60px;
          -webkit-filter: drop-shadow(6px 6px #0000005e);
          filter: drop-shadow(6px 6px #0000005e);
        }
      }
    }
    #sun {
      text-align: center;
      #sunrise,
      #sunset {
        width: 50%;
        margin-left: 1rem;
      }
      p {
        width: 100px;
        font-size: 1.5rem;
        text-shadow: 3px 3px #0000005e;
        box-shadow: 5px 5px #0000005e;
      }
    }
    #statusWeather {
      max-width: 330px;
      margin: auto;
      margin-bottom: 0.6rem;
      text-shadow: 3px 3px #0000005e;
      font-size: 1.3rem;
      grid-template-columns: 24% 32% 40%;
      #wind,
      #humidity,
      #pressure {
        box-shadow: 5px 5px #0000005e;
      }
    }
  }
}
</style>
