<template>
  <div>
    <header class="flex  justify-between">
      <div class="flex  items-center">
        <div class="w-10  h-10  -ml-5  mr-3  border-2  border-green-500  rounded-full  bg-gray-800">

        </div>

        <h4 class="text-sm">{{ char.name }}</h4>
      </div>

      <div class="flex  flex-col  items-end  space-y-2">
        <div class="flex  space-x-3  text-center  text-xs  text-gray-200">
          <span class="flex  items-center  justify-center "><HeartIcon class="block  h-4  w-4"/> <b class="ml-1">{{ char.stats.healthy.hp }}</b></span>
          <span class="flex  items-center  justify-center "><ChevronDoubleRightIcon class="block  h-4  w-4"/> <b class="ml-1  uppercase">{{ char.stats.healthy.speed }}</b></span>
          <span class="flex  items-center  justify-center "><SwitchVerticalIcon class="block  h-4  w-4"/> <b class="ml-1">{{ char.stats.healthy.size }}</b></span>
        </div>

        <div class="flex  items-center  space-x-2  text-center  text-xs  font-bold  text-black">
          <span class="text-gray-300  uppercase  tracking-widest">Def:</span>
          <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-red-500">{{ char.stats.healthy.physical }}</span>
          <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-yellow-400">{{ char.stats.healthy.energy }}</span>
          <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-blue-400">{{ char.stats.healthy.mystic }}</span>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
import { HeartIcon, ChevronDoubleRightIcon, SwitchVerticalIcon } from '@heroicons/vue/solid'

export default {
  props: ['char'],
  components: { HeartIcon, ChevronDoubleRightIcon, SwitchVerticalIcon }
}
</script>