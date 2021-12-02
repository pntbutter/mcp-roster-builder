<template>
  <div class="max-w-2xl  mx-auto">
    <div class="md:flex  items-center  justify-center  p-4  lg:py-8">
      <input v-model="opponentName" type="text" class="w-full  md:w-auto  mr-3  py-1  px-3  rounded-sm  bg-gray-800  text-gray-50  text-center  md:text-left" placeholder="Opponent name">
      <button @click="newBattle" class="btn  w-full  md:w-auto  mt-2  md:mt-0">
          New Battle
      </button>
    </div>
    
    <div class="space-y-2">
      <div v-for="(battle, index) in battles" :key="battle.date" class="flex  items-center  justify-between  py-3  pl-4  pr-3  rounded-md  transition-all  hover:shadow-xl  hover:ring-1  ring-pink-800" :class="{ 'bg-gray-800': activeBattle == index }">
        <div>
          You vs. {{ battle.opponent.name }}
          <small class="block  opacity-50">{{ prettyDate(battle.date) }}</small>
        </div>

        <div class="flex  items-center">
          <NuxtLink to="/roster" @click="activeBattle = index" class="btn">
            Select
          </NuxtLink>

          <button @click="battles.splice(index, 1)" class="ml-2  p-2  text-pink-600  hover:text-pink-800">
            <TrashIcon class="block  h-5  w-5"/>
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import { TrashIcon } from '@heroicons/vue/outline'

export default {
  data() {
    return {
      opponentName: '',
      battles: [],
      activeBattle: null
    }
  },
  methods: {
    newBattle() {
      this.battles.unshift({
        date: new Date(),
        you: {
          characters: [],
          tactics: []
        },
        opponent: {
          name: this.opponentName || 'Unnamed',
          characters: [],
          tactics: []
        }
      })
      this.opponentName = ''
    },
    prettyDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString()
    }
  },
  mounted() {
    this.battles = JSON.parse(localStorage.getItem("battles")) || []
    this.activeBattle = localStorage.getItem("activeBattle") || null
  },
  watch: {
    battles: {
      handler(val) {
        localStorage.setItem("battles", JSON.stringify(val));
      },
      deep: true
    },
    activeBattle(val) {
      localStorage.setItem("activeBattle", val)
    }
  },
  components: { TrashIcon }
}
</script>