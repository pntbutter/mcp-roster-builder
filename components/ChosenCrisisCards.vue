<script setup>
const crisisCards = useCrisisCards()
</script>

<template>
  <div>
    <div v-for="(card, index) in crisisCards" :key="index">
      {{ card.name }}
    </div>
  </div>
</template>
