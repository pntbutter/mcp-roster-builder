<script setup>
const { data } = await useFetch('/api/crisis-cards')
const crisisCards = useCrisisCards()
</script>

<template>
  <div class="card">
    <header class="flex  items-center  pt-3  px-3  mb-4">
      <h2 class="w-1/3  text-sm  lg:text-1xl  text-gray-500  font-weight-black  uppercase  tracking-widest">
        Crisis
      </h2>

      <input v-model="search" class="block  w-2/3  py-2  px-4  rounded  bg-gray-900  text-gray-100  lg:text-sm" type="text" placeholder="Search">
    </header>

    <ul class="space-y-0.5">
      <li v-for="(card, index) in data" :key="index" :class="[crisisCards.includes(card) ? 'opacity-20  pointer-events-none' : '' ]" class="flex  items-center  px-3  py-2  rounded-md  transition-all  hover:bg-gray-900">
        <span class="mr-auto">
          {{ card.name }}
        </span>

        <div class="flex  items-center  space-x-2">
          <button @click="crisisCards.push(card)" class="rounded-full  hover:bg-pink-900">
            <PlusCircleIcon class="block  h-6  w-6"/>
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { PlusCircleIcon } from '@heroicons/vue/outline'

export default {
  data() {
    return {
      search: ''
    }
  },
  components: { PlusCircleIcon }
}
</script>
