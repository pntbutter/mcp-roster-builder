<script setup>
const { data } = await useFetch('/api/stat-cards')
</script>

<template>
  <div>
    <input v-model="search" class="block  w-full  my-2  py-2  px-4  rounded  bg-gray-800  text-gray-100" type="text" placeholder="Search (name, alias, affiliation, threat)">

    <ul class="py-2  md:py-4  space-y-3  md:space-y-4">
      <li v-for="(card, index) in filteredCards" :key="index" class="flex  flex-wrap  md:items-center  rounded-md  transition-all">
        <span :title="card.alias">
          <b class="mr-2">{{ card.threat }}</b>
          <span>
            {{ card.name }}
            <span class="block  md:inline  md:ml-2  text-sm  text-gray-400" v-if="card.affiliation.length">
              <span v-for="(aff, index) in card.affiliation" :key="index">
                {{ aff }}<span v-if="!index == card.affiliation.length - 1">, </span>
              </span>
            </span>
            <span v-else class="block  md:inline  md:ml-2  text-sm  text-gray-400">
              No affiliations
            </span>
          </span>
        </span>

        <div class="flex  items-center  justify-end  space-x-2  ml-auto">
          <span class="hidden  md:block  text-xs  opacity-50  font-weight-bold  tracking-widest  uppercase">
            Add to:
          </span>

          <button @click="$attrs.you.characters.push(card)" v-if="$attrs.you" class="btn" :class="[$attrs.you.characters.includes(card) ? 'opacity-20  pointer-events-none' : '' ]">
            You
          </button>

          <button @click="$attrs.opponent.characters.push(card)" v-if="$attrs.opponent" class="btn" :class="[$attrs.opponent.characters.includes(card) ? 'opacity-20  pointer-events-none' : '' ]">
            {{ $attrs.opponent.name }}
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: ''
    }
  },
  computed: {
    filteredCards() {
      let cards = this.data
      let search = this.search.toLowerCase()

      if (this.search != '' && this.search) {
        cards = cards.filter((card) => {
          
          if (card.name.toLowerCase().includes(search) || 
              card.alias.toLowerCase().includes(search) || 
              card.affiliation.join().toLowerCase().includes(search) || 
              card.threat == search) {
            return true;
          } else {
            return false;
          }

        })
      }

      cards.sort(function(a, b) {
        var textA = a.name.toUpperCase();
        var textB = b.name.toUpperCase();
        return (textA < textB) ? -1 : (textA > textB) ? 1 : 0;
      });

      return cards
    }
  }
}
</script>
