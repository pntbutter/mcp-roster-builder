<template>
  <div>
    <header class="flex  justify-between">
      <div class="flex  items-center">
        <Menu as="div" class="relative">
          <MenuButton class="relative  overflow-hidden  w-10  h-10  -ml-7  mr-3  border-2  rounded-full  bg-gray-800  focus:outline-none" :class="portraitState">
            <img v-if="char.avatar && char.state.status != 'damaged'" :src="'/avatars/' + char.avatar.healthy" class="w-10">
            <img v-else-if="char.avatar && char.state.status == 'damaged'" :src="'/avatars/' + char.avatar.damaged" class="w-10">
            <XIcon v-if="char.state.status == 'ko'" class="absolute  inset-0  text-gray-900" />
          </MenuButton>

          <transition
            enter-active-class="transition duration-100 ease-out"
            enter-from-class="transform scale-95 opacity-0"
            enter-to-class="transform scale-100 opacity-100"
            leave-active-class="transition duration-75 ease-out"
            leave-from-class="transform scale-100 opacity-100"
            leave-to-class="transform scale-95 opacity-0"
          >
            <MenuItems class="absolute  z-30  top-full  left-0  p-2  rounded-md  border-tl-none  bg-gray-900  shadow-xl  space-y-2  text-sm  focus:ring-0">
              <MenuItem>
                <button @click="char.state.status = 'healthy'" class="w-full  py-2  px-6  rounded-sm" :class="[char.state.status == 'healthy' ? 'bg-pink-900' : 'bg-gray-800']">
                  Healthy
                </button>
              </MenuItem>
              <MenuItem>
                <button @click="char.state.status = 'dazed'" class="w-full  py-2  px-6  rounded-sm" :class="[char.state.status == 'dazed' ? 'bg-pink-900' : 'bg-gray-800']">
                  Dazed
                </button>
              </MenuItem>
              <MenuItem>
                <button @click="char.state.status = 'damaged'" class="w-full  py-2  px-6  rounded-sm" :class="[char.state.status == 'damaged' ? 'bg-pink-900' : 'bg-gray-800']">
                  Damaged
                </button>
              </MenuItem>
              <MenuItem>
                <button @click="char.state.status = 'ko'" class="w-full  py-2  px-6  rounded-sm" :class="[char.state.status == 'ko' ? 'bg-pink-900' : 'bg-gray-800']">
                  KO'd
                </button>
              </MenuItem>
            </MenuItems>
          </transition>
        </Menu>

        <h4 class="text-sm  font-bold">
          {{ char.name }}
          <div v-if="char.state.conditions.length" class="flex flex-wrap text-mini font-normal">
            <span v-for="cond in char.state.conditions" :key="cond" class="mr-1">{{ cond }}</span>
          </div>
        </h4>
      </div>

      <div class="flex  flex-col  items-end  space-y-2">
        <div class="flex  space-x-3  text-center  text-xs  text-gray-200">
          <span class="flex  items-center  justify-center  uppercase">
            <HeartIcon class="block  h-4  w-4  mr-1"/>
            <b v-if="char.state.status != 'damaged'">{{ char.stats.healthy.hp }}</b>
            <b v-else>{{ char.stats.damaged.hp }}</b>
          </span>
          <span class="flex  items-center  justify-center  uppercase">
            <ChevronDoubleRightIcon class="block  h-4  w-4  mr-1"/>
            <b v-if="char.state.status != 'damaged'">{{ char.stats.healthy.speed }}</b>
            <b v-else>{{ char.stats.damaged.speed }}</b>
          </span>
          <span class="flex  items-center  justify-center ">
            <SwitchVerticalIcon class="block  h-4  w-4  mr-1"/>
            <b v-if="char.state.status != 'damaged'">{{ char.stats.healthy.size }}</b>
            <b v-else>{{ char.stats.damaged.size }}</b>
            </span>
        </div>

        <div class="flex  items-center  space-x-2  text-center  text-xs  font-bold  text-black">
          <span class="text-gray-300  uppercase  tracking-widest">Def:</span>
          <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-red-500">{{ char.stats.healthy.physical }}</span>
          <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-yellow-400">{{ char.stats.healthy.energy }}</span>
          <span class="flex  items-center  justify-center  w-5  h-5  rounded-full  bg-blue-500">{{ char.stats.healthy.mystic }}</span>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
import { HeartIcon, ChevronDoubleRightIcon, SwitchVerticalIcon, XIcon } from '@heroicons/vue/solid'
import { Menu, MenuButton, MenuItems, MenuItem } from "@headlessui/vue";

export default {
  props: ['char'],
  computed: {
    portraitState() {
      return {
        'border-green-600 border-opacity-50': this.char.state.status == 'healthy',
        'border-purple-500 border-opacity-50': this.char.state.status == 'dazed',
        'border-red-600 border-opacity-50': this.char.state.status == 'damaged',
        'border-gray-900  opacity-50': this.char.state.status == 'ko'
      }
    }
  },
  components: {
    HeartIcon,
    ChevronDoubleRightIcon,
    SwitchVerticalIcon,
    XIcon,
    Menu,
    MenuButton,
    MenuItems,
    MenuItem
  }
}
</script>

<style>
.text-mini {
  font-size: .6rem;
}
</style>