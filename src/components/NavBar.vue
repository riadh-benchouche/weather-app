<script setup lang="ts">
import {Disclosure, DisclosureButton, DisclosurePanel} from '@headlessui/vue'
import {MagnifyingGlassIcon, MapPinIcon} from '@heroicons/vue/20/solid'
import {Bars3Icon, BellIcon, XMarkIcon} from '@heroicons/vue/24/outline'
import {reactive} from "vue";
import axios from "axios";
import MainComponent from "./MainComponent.vue";

const state = reactive({
  search: '' as string,
  loading: false as boolean,
  city: {} as string,
  weather: {} as object,
});
const OpenWeatherApiKey = import.meta.env.VITE_OPENWEATHER_API_KEY

const getWeatherByCity = async () => {
  await axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${state.search}&APPID=${OpenWeatherApiKey}`)
      .then((res: any) => {
        state.weather = res.data.list
        state.city = res.data.city
      })
      .catch((err: any) => console.error(err))
}
const getWeatherByPosition = () => {
  state.loading = true
  navigator.geolocation.getCurrentPosition(async (position) => {
    const {latitude, longitude} = position.coords;
    await axios.get(`https://api.openweathermap.org/data/2.5/forecast?lat=${latitude}&lon=${longitude}&APPID=${OpenWeatherApiKey}`)
        .then((res: any) => {
          state.weather = res.data.list
          state.city = res.data.city
          setTimeout(() => {
            state.loading = false
          }, 1000)
        })
        .catch((err: any) => console.error(err));
  })
};


const navigation = [
  {name: 'Weather', href: '#', current: true},
  {name: 'Favorites', href: '#', current: false},
]
</script>
<template>
  <div>
    <div class="bg-gray pb-32 ">
      <Disclosure as="nav" class="" v-slot="{ open }">
        <div class="mx-auto max-w-7xl px-2 sm:px-4 lg:px-8">
          <div class="relative flex h-16 items-center justify-between">
            <div class="flex items-center px-2 lg:px-0">
              <div class="flex-shrink-0">
                <img class="block h-8 w-8"
                     src="https://upload.wikimedia.org/wikipedia/commons/b/bf/Circle-icons-weather.svg"
                     alt="Your Company"/>
              </div>
              <div class="hidden lg:ml-10 lg:block">
                <div class="flex space-x-4">
                  <a v-for="item in navigation" :key="item.name" :href="item.href"
                     :class="[item.current ? 'bg-indigo-700 text-white' : 'text-white hover:bg-indigo-500 hover:bg-opacity-75', 'rounded-md py-2 px-3 text-sm font-medium']"
                     :aria-current="item.current ? 'page' : undefined">{{ item.name }}</a>
                </div>
              </div>
            </div>
            <div class="flex lg:hidden">
              <!-- Mobile menu button -->
              <DisclosureButton
                  class="inline-flex items-center justify-center rounded-md bg-indigo-600 p-2 text-indigo-200 hover:bg-indigo-500 hover:bg-opacity-75 hover:text-white focus:outline-none focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-indigo-600">
                <span class="sr-only">Open main menu</span>
                <Bars3Icon v-if="!open" class="block h-6 w-6" aria-hidden="true"/>
                <XMarkIcon v-else class="block h-6 w-6" aria-hidden="true"/>
              </DisclosureButton>
            </div>
          </div>
        </div>
        <DisclosurePanel class="lg:hidden">
          <div class="space-y-1 px-2 pt-2 pb-3">
            <DisclosureButton v-for="item in navigation" :key="item.name" as="a" :href="item.href"
                              :class="[item.current ? 'bg-indigo-700 text-white' : 'text-white hover:bg-indigo-500 hover:bg-opacity-75', 'block rounded-md py-2 px-3 text-base font-medium']"
                              :aria-current="item.current ? 'page' : undefined">{{ item.name }}
            </DisclosureButton>
          </div>
        </DisclosurePanel>
      </Disclosure>
      <header class="flex py-10 justify-between mx-20">
        <div class="max-w-full px-4 sm:px-6 lg:px-8">
          <h1 class="text-3xl font-bold tracking-tight text-white">Weather</h1>
        </div>
        <div class="flex flex-1">
          <div class="flex flex-1 justify-center  ">
            <div class="w-full mr-2">
              <label for="search" class="sr-only">Search</label>
              <div class="relative text-gray-400 focus-within:text-gray-600">
                <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
                  <MagnifyingGlassIcon class="h-5 w-5" aria-hidden="true"/>
                </div>
                <input id="search"
                       v-model="state.search"
                       @change="getWeatherByCity"
                       class="block w-full rounded-md border border-transparent bg-white py-2 pl-10 pr-3 leading-5 text-gray-900 placeholder-gray-500 focus:border-white focus:outline-none focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-indigo-600 sm:text-sm"
                       placeholder="Search" type="search" name="search"/>
              </div>
            </div>
            <button type="button"
                    @click="getWeatherByPosition"
                    class="text-white bg-white hover:bg-gray-300 font-medium rounded-lg text-sm p-2.5 text-center inline-flex items-center pr-2">
              <MapPinIcon v-if="!state.loading" class="h-5 w-5 text-black" aria-hidden="true"/>
              <svg v-else aria-hidden="true" role="status" class="inline w-4 h-4 text-black animate-spin" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="#E5E7EB"/>
                <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentColor"/>
              </svg>
            </button>
          </div>
        </div>
      </header>
    </div>
    <MainComponent :weather="state.weather" :city="state.city"/>
  </div>
</template>

