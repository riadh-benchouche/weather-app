<script setup>
import WeatherComponent from "./WeatherComponent.vue";
import CityComponent from "./CityComponent.vue";

const props = defineProps()

</script>
<template>
  <main class="lg:flex lg:flex-row -mt-32 lg:mx-20">
    <WeatherComponent/>
    <CityComponent/>
  </main>
</template>

