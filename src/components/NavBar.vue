<script setup>
import {Disclosure, DisclosureButton, DisclosurePanel} from '@headlessui/vue'
import {MagnifyingGlassIcon} from '@heroicons/vue/20/solid'
import {Bars3Icon, BellIcon, XMarkIcon} from '@heroicons/vue/24/outline'
import InfoComponent from "./InfoComponent.vue";

const navigation = [
  {name: 'Weather', href: '#', current: true},
  {name: 'Favorites', href: '#', current: false},
]
</script>
<template>
  <div class="h-screen">
    <div class="bg-gray pb-32 ">
      <Disclosure as="nav" class="" v-slot="{ open }">
        <div class="mx-auto max-w-7xl px-2 sm:px-4 lg:px-8">
          <div class="relative flex h-16 items-center justify-between">
            <div class="flex items-center px-2 lg:px-0">
              <div class="flex-shrink-0">
                <img class="block h-8 w-8" src="https://upload.wikimedia.org/wikipedia/commons/b/bf/Circle-icons-weather.svg"
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
            <div class="flex flex-1 justify-center px-2 lg:ml-6 lg:justify-end">
              <div class="w-full max-w-lg lg:max-w-xs">
                <label for="search" class="sr-only">Search</label>
                <div class="relative text-gray-400 focus-within:text-gray-600">
                  <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
                    <MagnifyingGlassIcon class="h-5 w-5" aria-hidden="true"/>
                  </div>
                  <input id="search"
                         class="block w-full rounded-md border border-transparent bg-white py-2 pl-10 pr-3 leading-5 text-gray-900 placeholder-gray-500 focus:border-white focus:outline-none focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-indigo-600 sm:text-sm"
                         placeholder="Search" type="search" name="search"/>
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
            <div class="hidden lg:ml-4 lg:block">
              <div class="flex items-center">
                <button type="button"
                        class="flex-shrink-0 rounded-full bg-indigo-600 p-1 text-indigo-200 hover:text-white focus:outline-none focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-indigo-600">
                  <span class="sr-only">View notifications</span>
                  <BellIcon class="h-6 w-6" aria-hidden="true"/>
                </button>
              </div>
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
          <div class="border-t border-indigo-700 pt-4 pb-3">
            <div class="flex items-center px-5">
              <button type="button"
                      class="ml-auto flex-shrink-0 rounded-full bg-indigo-600 p-1 text-indigo-200 hover:text-white focus:outline-none focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-indigo-600">
                <span class="sr-only">View notifications</span>
                <BellIcon class="h-6 w-6" aria-hidden="true"/>
              </button>
            </div>
          </div>
        </DisclosurePanel>
      </Disclosure>
      <InfoComponent/>
    </div>
    <main class="lg:flex lg:flex-row -mt-32 lg:mx-20">
      <div class="lg:basis-3/5 sm:w-full px-4 pb-4 sm:px-6 lg:px-8">
        <div class="rounded-lg bg-dimGray px-5 py-6 shadow-2xl sm:px-6 ">
          <div class="h-96 rounded-lg border-gray-200"/>
        </div>
      </div>
      <div class="lg:basis-2/5 sm:w-full px-4  sm:px-6 lg:px-8">
        <div class="rounded-lg bg-dimGray px-5  pb-4 py-6 shadow-2xl sm:px-6 ">
          <div class="h-96 rounded-lg border-gray-200"/>
        </div>
      </div>
    </main>
  </div>
</template>

