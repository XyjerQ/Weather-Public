<script setup lang="ts">
import { ref, reactive } from "vue";
import * as apikey from "@/Api/apikey";
import displaySearch from "./displaySearch.vue";

const emit = defineEmits(["cords"]);

const townName = ref("");

interface searchObject {
  name: string;
  country: string;
  lat: number;
  lon: number;
}
const arraySearch = reactive<Array<searchObject>>([]);

const fetchSearch = () => {
  if (townName.value != "") {
    const url = `https://api.openweathermap.org/geo/1.0/direct?q=${townName.value}&limit=5&appid=${apikey.key}`;
    fetch(url)
      .then((response) => response.json())
      .then((response) => {
        console.log(response);
        arraySearch.length = 0;
        for (let i = 0; i < response.length; i++) {
          arraySearch.push(response[i]);
        }
        console.log(arraySearch);
      });
  }
};
const cords = (passedCords: object) => {
  emit("cords", passedCords);
  arraySearch.length = 0;
  townName.value = "";
};
</script>

<template>
  <div id="search">
    <input
      type="text"
      placeholder="Search city"
      v-model="townName"
      @keyup.enter="fetchSearch()"
    />
    <button @click="fetchSearch()">Search</button>
  </div>
  <displaySearch
    v-for="search in arraySearch"
    :key="search.lat"
    :name="search.name"
    :country="search.country"
    :lat="search.lat"
    :lon="search.lon"
    @cords="cords"
  />
</template>

<style lang="scss" scoped>
#search {
  display: flex;
  justify-content: center;
  margin-top: 3rem;
  margin-bottom: 0.2rem;

  input {
    width: 600px;
    height: 50px;
    font-size: 1.8rem;
    background-color: rgba(255, 255, 255, 0.438);
    border-top-left-radius: 20px;
    border-bottom-left-radius: 20px;
    border: 0px;
    color: rgb(255, 255, 255);
    text-shadow: 3px 3px #0000005e;
    padding-left: 1rem;
  }
  input:focus {
    outline: none;
  }
  button {
    width: 100px;
    background-color: rgba(255, 255, 255, 0.438);
    border-top-right-radius: 20px;
    border-bottom-right-radius: 20px;
    border: 0px;
    border-left: 2px solid rgba(77, 77, 77, 0.185);
    color: rgb(255, 255, 255);
    text-shadow: 3px 3px #0000005e;
    font-size: 1.5rem;
  }
  button:hover,
  input:hover {
    background-color: rgba(255, 255, 255, 0.589);
  }
}
@media only screen and (max-width: 750px) {
  #search {
    width: 90%;
    margin: auto;
    margin-top: 2rem;
    margin-bottom: 0.2rem;

    input {
      height: 40px;
      font-size: 1.3rem;
      text-shadow: 2px 2px #0000005e;
      padding-left: 1rem;
    }
    button {
      width: 80px;
      text-shadow: 2px 2px #0000005e;
      font-size: 1.1rem;
    }
  }
}
@media only screen and (max-width: 400px) {
  #search {
    margin: auto;
    margin-top: 2rem;
    margin-bottom: 0.2rem;

    input {
      width: 80%;
      height: 40px;
      font-size: 1.3rem;
      text-shadow: 2px 2px #0000005e;
      padding-left: 1rem;
    }
    button {
      width: 80px;
      text-shadow: 2px 2px #0000005e;
      font-size: 1.1rem;
    }
  }
}
</style>
