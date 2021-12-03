<template>
  <div v-if="battle" class="max-w-5xl  mx-auto">
    <div class="md:grid  md:grid-cols-2  gap-6  space-y-10  md:space-y-0  pt-6">
      <div v-if="battle.you">
        <h3 class="mb-4  title  md:text-left">You</h3>

        <ul v-if="battle.you.characters" class="space-y-2">
          <li v-for="(char, index) in battle.you.characters" class="card  disable-dbl-tap-zoom">
            <header class="flex  justify-between">
              <div>

                <h4>{{ char.name }}</h4>
              </div>

              <div class="flex  flex-col  items-end  space-y-2">
                <div class="flex  space-x-3  text-center  text-xs  text-gray-200">
                  <span class="flex  items-center  justify-center "><HeartIcon class="block  h-4  w-4"/> <b class="ml-1">{{ char.stats.healthy.hp }}</b></span>
                  <span class="flex  items-center  justify-center "><ChevronDoubleRightIcon class="block  h-4  w-4"/> <b class="ml-1  uppercase">{{ char.stats.healthy.speed }}</b></span>
                  <span class="flex  items-center  justify-center "><SwitchVerticalIcon class="block  h-4  w-4"/> <b class="ml-1">{{ char.stats.healthy.size }}</b></span>
                </div>

                <div class="flex  items-center  space-x-2  text-center  text-xs  font-bold  text-black">
                  <span class="text-gray-300  uppercase  tracking-widest">Def:</span>
                  <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-red-500">{{ char.stats.healthy.physical }}</span>
                  <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-yellow-400">{{ char.stats.healthy.energy }}</span>
                  <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-blue-400">{{ char.stats.healthy.mystic }}</span>
                </div>
              </div>
            </header>
            
            <footer class="flex  flex-wrap  space-x-6">
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