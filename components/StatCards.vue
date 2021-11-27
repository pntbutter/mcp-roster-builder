<script setup>
const { data } = await useFetch('/api/stat-cards')
const statCards = useStatCards()
</script>

<template>
  <div class="card">
    <header class="flex  items-center  pt-3  px-3  mb-4">
      <h2 class="w-1/3  text-sm  lg:text-1xl  text-gray-500  font-weight-black  uppercase  tracking-widest">
        Characters
      </h2>

      <input v-model="search" class="block  w-2/3  py-2  px-4  rounded  bg-gray-900  text-gray-100  lg:text-sm" type="text" placeholder="Search">
    </header>

    <ul class="space-y-0.5">
      <li v-for="(card, index) in filteredList" :key="index" :class="[statCards.includes(card) ? 'opacity-20  pointer-events-none' : '' ]" class="flex  items-center  px-3  py-2  rounded-md  transition-all  hover:bg-gray-900">
        <span class="mr-auto" :title="card.alias">
          <b class="mr-2">{{ card.threat }}</b>
          <span>
            {{ card.name }}
            <span class="ml-2  text-sm  text-gray-400">
              <span class="aff" v-for="(aff, index) in card.affiliation" :key="index">
                {{ aff }}<span v-if="!index == card.affiliation.length - 1">, </span>
              </span>
            </span>
          </span>
        </span>

        <div class="flex  items-center  space-x-2">
          <button @click="statCards.push(card)" class="  rounded-full  hover:bg-pink-900">
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
  computed: {
    filteredList() {
      let cards = this.data
      let search = this.search.toLowerCase()

      if (this.search != '' && this.search) {
        cards = cards.filter((card) => {
          
          if (card.name.toLowerCase().includes(search) || 
              card.alias.toLowerCase().includes(search) || 
              card.affiliation.join().toLowerCase().includes(search) || 
              card.threat == search) {
            return true;
          } else {
            return false;
          }

        })
      }

      return cards
    }
  },
  components: { PlusCircleIcon }
}
</script>

<style scoped>
.aff + .aff {
  content: ', ';
}
</style>