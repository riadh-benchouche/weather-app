<script setup lang="ts">
import {reactive} from "vue";
import axios from "axios";
import NavBar from "./components/NavBar.vue";

const state = reactive({
  search: '' as string,
  city: {} as string,
  weather: {} as object,
});
const OpenWeatherApiKey = import.meta.env.VITE_OPENWEATHER_API_KEY

const getWeatherByCity = async () => {
  await axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${state.search}&APPID=${OpenWeatherApiKey}`)
      .then((res: any) => state.weather = res.data)
      .catch((err: any) => console.error(err))
}
const getWeatherByPosition = () => {
  navigator.geolocation.getCurrentPosition(async (position) => {
    const {latitude, longitude} = position.coords;
    await axios.get(`https://api.openweathermap.org/data/2.5/forecast?lat=${latitude}&lon=${longitude}&APPID=${OpenWeatherApiKey}`)
        .then((res: any) => state.weather = res.data)
        .catch((err: any) => console.error(err))
  })
};

</script>

<template>
  <div>
    <NavBar/>
  </div>
</template>
