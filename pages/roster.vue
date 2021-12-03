<script setup>
const { data } = await useFetch('/api/stat-cards')
</script>

<template>
  <div v-if="battle" class="max-w-5xl  mx-auto">
    <StatCards v-if="battle" :battle="battle" class="max-w-3xl  mx-auto  mb-8  md:mb-16" />

    <div class="md:grid  md:grid-cols-2  gap-6  space-y-10  md:space-y-0">
      <div class="relative  p-6  card" v-if="battle.you">
        <h3 class="absolute  left-6  -top-3  uppercase  text-gray-400  font-weight-black  tracking-widest">
          You
        </h3>

        <ul v-if="battle.you.characters.length" class="py-2">
          <li v-for="(char, index) in battle.you.characters" :key="index" class="flex  items-center">
            <b class="mr-2">{{ char.threat }}</b>
            <span>{{ char.name }}</span>
            <button @click="battle.you.characters.splice(index, 1)" class="ml-auto  p-2  text-pink-600  hover:text-pink-800">
              <TrashIcon class="block  h-5  w-5"/>
            </button>
          </li>
        </ul>
        <div v-else class="opacity-50">
          Empty, add some characters
        </div>

        <span class="absolute  left-6  -bottom-3  font-weight-bold  text-gray-400  tracking-wide">
          {{ battle.you.characters.length }} / 10
        </span>
      </div>

      <div class="relative  p-6  card" v-if="battle.opponent">
        <input v-model="battle.opponent.name" type="text" class="block  bg-transparent  absolute  left-6  -top-3  uppercase  text-gray-400  font-weight-black  tracking-widest" placeholder="Opponent">
        
        <ul v-if="battle.opponent.characters.length" class="py-2">
          <li v-for="(char, index) in battle.opponent.characters" :key="index" class="flex  items-center">
            <b class="mr-2">{{ char.threat }}</b>
            <span>{{ char.name }}</span>
            <button @click="battle.opponent.characters.splice(index, 1)" class="ml-auto  p-2  text-pink-600  hover:text-pink-800">
              <TrashIcon class="block  h-5  w-5"/>
            </button>
          </li>
        </ul>
        <div v-else class="opacity-50">
          Empty, add some characters
        </div>

        <span class="absolute  left-6  -bottom-3  font-weight-bold  text-gray-400  tracking-wide">
          {{ battle.opponent.characters.length }} / 10
        </span>
      </div>
    </div>

    <div class="pt-16  pb-24  text-center">
      <NuxtLink to="/play" class="btn">Play</NuxtLink>
    </div>

  </div>
  <div v-else class="p-6  text-center">
    <NuxtLink to="/">You need to select a battle first</NuxtLink>
  </div>
</template>

<script>
import { TrashIcon } from '@heroicons/vue/outline'

export default {
  data() {
    return {
      battles: [],
      battle: {},
      activeBattle: [],
      search: '',
    }
  },
  methods: {
    
  },
  mounted() {
    this.battles = JSON.parse(localStorage.getItem("battles")) || []
    this.activeBattle = localStorage.getItem("activeBattle") || null
    this.battle = this.battles[this.activeBattle]
  },
  watch: {
    battles: {
      deep: true,
      handler(val) {
        localStorage.setItem("battles", JSON.stringify(val));
      }
    }
  },
  components: { TrashIcon }
}
</script>

<style>
  .container {
    max-width: 1000px;
  }
</style>