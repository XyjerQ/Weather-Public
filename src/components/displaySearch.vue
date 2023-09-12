<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits(["cords"]);

const props = defineProps({
  name: { type: String, required: true },
  country: { type: String, required: true },
  lat: { type: Number, required: true },
  lon: { type: Number, required: true },
});

const cords = ref<object>({});

const passDetails = () => {
  cords.value = {
    lat: props.lat.toFixed(2),
    lon: props.lon.toFixed(2),
  };
  emit("cords", cords.value);
};
</script>

<template>
  <div id="searchResults">
    <div id="displayResults" @click="passDetails()">
      <div id="name">{{ props.name }}, {{ props.country }}</div>
      <div id="lat">lat: {{ props.lat.toFixed(2) }}</div>
      <div id="lon">lon: {{ props.lon.toFixed(2) }}</div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
#searchResults {
  display: flex;
  justify-content: center;
  #displayResults {
    width: 700px;
    display: grid;
    grid-template-columns: 52% 22% 22%;
    grid-gap: 10px;
    text-align: left;
    font-size: 1.8rem;
    background-color: rgba(255, 255, 255, 0.438);
    border-radius: 20px;
    color: rgb(255, 255, 255);
    text-shadow: 3px 3px #0000005e;
    padding-left: 1rem;
    margin: 0.2rem;
    height: 50px;
    padding-top: 3px;
  }
  #displayResults:hover {
    background-color: rgba(255, 255, 255, 0.589);
    cursor: pointer;
  }
  #lat,
  #lon {
    padding-left: 0.3rem;
    border-left: 2px solid rgba(77, 77, 77, 0.185);
  }
}
@media only screen and (max-width: 750px) {
  #searchResults {
    width: 90%;
    margin: auto;
    #displayResults {
      font-size: 1.3rem;
      text-shadow: 2px 2px #0000005e;
      height: 40px;
      grid-template-columns: 100%;
    }
    #lat,
    #lon {
      display: none;
    }
  }
}
</style>
