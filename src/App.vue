<script setup lang="ts">
import {reactive} from "vue";
import axios from "axios";
import NavBar from "./components/NavBar.vue";

const state = reactive({
  search: '' as string,
  city: {} as string,
  weather: {} as object,
});
const ninjaAPI = 'W6Huu1UgSRrOrSo7cy3OVw==N0GKEeDOIiW6FxG9'
const LocationIqToken = 'pk.e7eabc335b4a6afcc48956d1d40ff0ff'

const config = {
  headers:{
    'X-Api-Key': ninjaAPI
  }
};
const getWeatherByCity = async () => {
  await axios.get(`https://api.api-ninjas.com/v1/weather?city=${state.search}`, config)
      .then((res: any)=> state.weather = res.data)
      .catch((err: any)=> console.log(err))
}
const getWeatherByPosition = () => {
  navigator.geolocation.getCurrentPosition(async (position) => {
    const {latitude, longitude} = position.coords;
    await axios.get(`https://api.api-ninjas.com/v1/weather?lat=${latitude}&lon=${longitude}`, config)
        .then((res: any)=> state.weather = res.data)
        .catch((err: any)=> console.log(err))
    await axios.get(`https://us1.locationiq.com/v1/reverse?key=${LocationIqToken}&lat=${latitude}&lon=${longitude}&format=json`)
        .then((res: any)=> state.city = res.data)
        .catch((err: any)=> console.log(err))
  })
};

</script>

<template>
  <div>
    <NavBar/>
  </div>
</template>
