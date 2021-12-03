<template>
  <div v-if="battle" class="max-w-5xl  mx-auto">
    <div class="md:grid  md:grid-cols-2  gap-6  space-y-10  md:space-y-0  md:pt-6">
      <div v-if="battle.you">
        <h3 class="mb-4  title  md:text-left">You</h3>

        <ul v-if="battle.you.characters" class="space-y-2">
          <li v-for="(char, index) in battle.you.characters" class="card  disable-dbl-tap-zoom">
            <PlayCharHeader :char="char" />
            
            <footer class="flex  flex-wrap  mt-4  space-x-6">
              <div class="flex  items-center">
                <button @click="decreasePower(char)" class="flex  items-center  justify-center  w-8  h-8  rounded-full  bg-green-700" :class="{ 'opacity-50': char.state.power <= 0 }">
                  <MinusSmIcon class="block  h-5  w-5"/>
                </button>
                <span class="block  w-10  text-xl  font-bold  text-center">{{ char.state.power }}</span>
                <button @click="increasePower(char)" class="flex  items-center  justify-center  w-8  h-8  rounded-full  bg-green-700" :class="{ 'opacity-50': char.state.power >= 10 }">
                  <PlusSmIcon class="block  h-5  w-5"/>
                </button>
              </div>

              <div class="flex  items-center">
                <button @click="decreaseDamage(char)" class="flex  items-center  justify-center  w-8  h-8  rounded-full  bg-red-800" :class="{ 'opacity-50': char.state.damage <= 0 }">
                  <MinusSmIcon class="block  h-5  w-5"/>
                </button>
                <span class="block  w-10  text-xl  font-bold  text-center">
                  {{ char.state.damage }}
                </span>
                <button @click="increaseDamage(char)" class="flex  items-center  justify-center  w-8  h-8  rounded-full  bg-red-800">
                  <PlusSmIcon class="block  h-5  w-5"/>
                </button>
              </div>
            </footer>
          </li>
        </ul>
      </div>

      <div v-if="battle.opponent">
        <h3 class="mb-4  title  md:text-left">{{ battle.opponent.name }}</h3>
      </div>
    </div>
  </div>
  <div v-else class="p-6  text-center">
    <NuxtLink to="/">You need to select a battle first</NuxtLink>
  </div>
</template>

<script>
import { HeartIcon, ChevronDoubleRightIcon, SwitchVerticalIcon, PlusSmIcon, MinusSmIcon } from '@heroicons/vue/solid'

export default {
  data() {
    return {
      battles: [],
      battle: {},
      activeBattle: [],
    }
  },
  methods: {
    increasePower(char) {
      if (char.state.power < 10) {
        char.state.power++
      }
    },
    decreasePower(char) {
      if (char.state.power > 0) {
        char.state.power--
      }
    },
    increaseDamage(char) {
      if (char.state.damage < 10) {
        char.state.damage++
      }
    },
    decreaseDamage(char) {
      if (char.state.damage > 0) {
        char.state.damage--
      }
    }
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
  components: { HeartIcon, ChevronDoubleRightIcon, SwitchVerticalIcon, PlusSmIcon, MinusSmIcon }
}
</script>

<style>
.disable-dbl-tap-zoom {
  touch-action: manipulation;
}
</style>