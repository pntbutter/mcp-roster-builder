<script>
export default defineComponent({
  setup() {
    const statCards = useStatCards()

    return { statCards }
  },
  methods: {
    removeStatCard(i) {
      this.statCards.splice(i, 1)
    },
    empty() {
      this.statCards = []
    }
  }
})
</script>

<template>
  <div>
    <ul>
      <li v-for="(card, index) in statCards" :key="index" @click="removeStatCard(index)">
        {{ card.name }}
      </li>
      <li @click="empty" class="mt-4">Töm</li>
    </ul>
  </div>
</template>
