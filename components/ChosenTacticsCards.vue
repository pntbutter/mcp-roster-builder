<script setup>
const tacticsCards = useTacticsCards()
</script>

<template>
  <div>
    <div v-for="(card, index) in tacticsCards" :key="index" @click="tacticsCards.splice(index, 1)">
      {{ card.name }}
    </div>
  </div>
</template>
