<template>
  <div class="max-w-5xl  mx-auto  md:grid  md:grid-cols-2  gap-6">
    <div class="p-6  card" v-if="battle.you">
      <h3>You</h3>
      
      {{ battle.you }}

      <StatCards @add-character="onAddYouCharacter" player="you" />
    </div>

    <div class="p-6  card" v-if="battle.opponent">
      <input v-model="battle.opponent.name" type="text" class="block  w-full  bg-transparent" placeholder="Opponent">
      
      {{ battle.opponent }}

      <StatCards @add-character="onAddOpponentCharacter" player="opponent" />
    </div>

    
  </div>
</template>

<script>
export default {
  data() {
    return {
      battles: [],
      battle: {},
      activeBattle: []
    }
  },
  methods: {
    onAddYouCharacter(character) {
      this.battle.you.characters.push(character)
    },
    onAddOpponentCharacter(character) {
      this.battle.opponent.characters.push(character)
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
  .container {
    max-width: 1000px;
  }
</style>