<script setup>
const activeBattleIndex = useActiveBattleIndex()
</script>

<template>
  <div>
    {{ battle[this.activeBattleIndex] }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      battles: []
    }
  },
  computed: {
    battle() {
      return this.battles[this.activeBattleIndex]
    }
  },
  methods: {
    
  },
  mounted() {
    this.battles = JSON.parse(localStorage.getItem("battles")) || []
  },
  watch: {
    battles: {
      handler(val) {
        localStorage.setItem("battles", JSON.stringify(val));
      },
      deep: true
    }
  }
}
</script>