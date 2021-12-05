<template>
  <div v-if="battle" class="max-w-5xl  mx-auto">
    <div class="md:grid  md:grid-cols-2  gap-6  space-y-10  md:space-y-0  md:pt-6">
      <div v-if="battle.you" class="relative  z-10">
        <h3 class="mb-4  title  md:text-left">You</h3>

        <ul v-if="battle.you.characters" class="space-y-2">
          <li v-for="(char, index) in battle.you.characters" class="card  relative  border-1  disable-dbl-tap-zoom" :class="stateBorderColor(char)">
            <PlayCharHeader :char="char" />
            
            <PlayCharFooter :char="char" />
          </li>
        </ul>
      </div>

      <div v-if="battle.opponent">
        <h3 class="mb-4  title  md:text-left">{{ battle.opponent.name }}</h3>

        <ul v-if="battle.opponent.characters" class="space-y-2">
          <li v-for="(char, index) in battle.opponent.characters" class="card  border-1  disable-dbl-tap-zoom" :class="stateBorderColor(char)">
            <PlayCharHeader :char="char" />
            
            <PlayCharFooter :char="char" />
          </li>
        </ul>
      </div>
    </div>
  </div>
  <div v-else class="p-6  text-center">
    <NuxtLink to="/">You need to select a battle first</NuxtLink>
  </div>
</template>

<script>
export default {
  data() {
    return {
      battles: [],
      battle: {},
      activeBattle: [],
    }
  },
  methods: {
    stateBorderColor(char) {
      return {
        'border-green-600 border-opacity-50': char.state.status == 'healthy',
        'border-purple-500 border-opacity-50': char.state.status == 'dazed',
        'border-red-600 border-opacity-50': char.state.status == 'damaged',
        'border-gray-900  filter  sepia  brightness-50': char.state.status == 'ko'
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
  }
}
</script>

<style>
.disable-dbl-tap-zoom {
  touch-action: manipulation;
}
</style>