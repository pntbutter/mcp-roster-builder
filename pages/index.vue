<template>
  <div>
    <div class="flex  items-center  justify-center  py-8">
      <input v-model="opponentName" type="text" class="mr-3  py-1  px-3  rounded-sm  bg-gray-800  text-gray-50" placeholder="Opponent name">
      <button @click="newBattle" class="py-2  px-4  rounded-sm  bg-gradient-to-b  from-pink-600  to-pink-900  hover:from-pink-600  hover:to-pink-700  text-xs  font-weight-bold  uppercase  tracking-widest">
          New Battle
      </button>
    </div>
    
    <div class="space-y-2">
      <div v-for="battle in battles" :key="battle.date" class="py-3  px-4  rounded-md  transition-all  hover:shadow-xl  hover:bg-gray-800">
        You vs. {{ battle.opponent.name }}
        <small class="block  opacity-50">{{ battle.date.toLocaleDateString('sv-SE') }}</small>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      opponentName: '',
      battles: []
    }
  },
  methods: {
    newBattle() {
      this.battles.unshift({
        date: new Date(),
        you: {
          characters: [],
          tactics: [],
          crisis: {
            extraction: [],
            secure: []
          }
        },
        opponent: {
          name: this.opponentName || 'Unnamed',
          characters: [],
          tactics: [],
          crisis: {
            extraction: [],
            secure: []
          }
        }
      })
      this.opponentName = ''
    }
  },
  mounted() {
    this.battles = JSON.parse(localStorage.getItem("battles")) || []
  },
  watch: {
    battles: {
      handler(val, oldVal) {
        localStorage.setItem("battles", JSON.stringify(val));
      },
      deep: true
    }
  }
}
</script>