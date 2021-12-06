<template>
  <footer class="flex flex-wrap justify-between mt-5">
    <div class="flex flex-wrap items-center">
      <button
        @click="decreasePower(char)"
        class="flex items-center justify-center w-8 h-8 rounded-full bg-gradient-to-b from-green-600 to-green-900"
        :class="{ 'opacity-50': char.state.power <= 0 }"
      >
        <MinusSmIcon class="block h-5 w-5" />
      </button>
      <span class="block w-10 text-xl font-bold text-center">{{ char.state.power }}</span>
      <button
        @click="increasePower(char)"
        class="flex items-center justify-center w-8 h-8 rounded-full bg-gradient-to-b from-green-600 to-green-900"
        :class="{ 'opacity-50': char.state.power >= 10 }"
      >
        <PlusSmIcon class="block h-5 w-5" />
      </button>
    </div>

    <div class="flex items-center">
      <button
        @click="decreaseDamage(char)"
        class="flex items-center justify-center w-8 h-8 rounded-full bg-gradient-to-b from-red-600 to-red-900"
        :class="{ 'opacity-50': char.state.damage <= 0 }"
      >
        <MinusSmIcon class="block h-5 w-5" />
      </button>
      <span class="block w-10 text-xl font-bold text-center">{{ char.state.damage }}</span>
      <button
        @click="increaseDamage(char)"
        class="flex items-center justify-center w-8 h-8 rounded-full bg-gradient-to-b from-red-600 to-red-900"
      >
        <PlusSmIcon class="block h-5 w-5" />
      </button>
    </div>

    <Popover class="">
      <PopoverButton class="btn-outline">
        Cond.
      </PopoverButton>

      <transition
        enter-active-class="transition duration-100 ease-out"
        enter-from-class="transform scale-95 opacity-0"
        enter-to-class="transform scale-100 opacity-100"
        leave-active-class="transition duration-75 ease-out"
        leave-from-class="transform scale-100 opacity-100"
        leave-to-class="transform scale-95 opacity-0"
      >
        <PopoverPanel  class="absolute  z-30  flex  flex-wrap  items-start  justify-start  bottom-0  right-0  w-full  h-full  p-1  rounded-md  bg-gray-900  shadow-xl    text-sm  focus:ring-0">
          <PopoverButton v-for="cond in conditions" :key="cond" @click="toggleCondition(char, cond)" class="flex-1  m-1  py-1  px-3  rounded-sm" :class="[char.state.conditions.includes(cond) ? 'bg-pink-900' : 'bg-gray-800']">
            {{ cond }}
          </PopoverButton>
          <PopoverButton @click="char.state.conditions = []" class="flex-1  m-1  py-1  px-3  rounded-sm">
            Remove all
          </PopoverButton>
        </PopoverPanel >
      </transition>
    </Popover >
  </footer>
</template>

<script>
import { PlusSmIcon, MinusSmIcon } from '@heroicons/vue/solid'
import { Popover, PopoverButton, PopoverPanel } from "@headlessui/vue";

export default {
  props: ['char'],
  data() {
    return {
      conditions: [
        'Bleed',
        'Hex',
        'Incinerate',
        'Judgment',
        'Poison',
        'Root',
        'Shock',
        'Slow',
        'Stagger',
        'Stun'
      ]
    }
  },
  methods: {
    increasePower(char) {
      // dont go over 10 power
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
      char.state.damage++

      // daze character if its healthy & takes enough damage
      if (char.state.status == 'healthy' && char.state.damage == char.stats.healthy.hp) {
        char.state.status = 'dazed'
        char.state.damage = 0
      }

      // if adding first damage when dazed, set status to damaged since player probably forgot
      if (char.state.status == 'dazed' && char.state.damage == 1) {
        char.state.status = 'damaged'
      }

      // ko character if its damaged & takes enough damage
      if (char.state.status == 'damaged' && char.state.damage == char.stats.damaged.hp) {
        char.state.status = 'ko'
        char.state.damage = 0
      }
    },
    decreaseDamage(char) {
      if (char.state.damage > 0) {
        char.state.damage--
      }
    },
    toggleCondition(char, cond) {
      // if condition already exists remove it, else add it
      if (char.state.conditions.includes(cond)) {
        const condIndex = (element) => element == cond;
        char.state.conditions.splice( char.state.conditions.findIndex(condIndex), 1)
      } else {
        char.state.conditions.push(cond)
      }
    }
  },
  components: { 
    PlusSmIcon, 
    MinusSmIcon, 
    Popover, 
    PopoverButton, 
    PopoverPanel
  }
}
</script>