<template>
  <footer class="flex flex-wrap mt-4 space-x-6">
    <div class="flex items-center">
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
  </footer>
</template>

<script>
import { PlusSmIcon, MinusSmIcon } from '@heroicons/vue/solid'

export default {
  props: ['char'],
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
    }
  },
  components: { PlusSmIcon, MinusSmIcon }
}
</script>