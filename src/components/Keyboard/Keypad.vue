<script setup lang="ts">
import { ref, toRefs } from 'vue';
import { useStore } from 'vuex';

const props = defineProps<{
  keyboardRow: {
    letter: string;
    state: 'out' | 'ball' | 'strike' | 'none';
  }[];
}>();
const { keyboardRow } = toRefs(props);
const keyRef = ref<{ [key: number]: Element }>([]);
const store = useStore();
const clickKey = (index: number, letter?: string) => {
  keyRef.value[index].classList.toggle('animate-wiggle');
  if (letter === undefined) return;
  store.dispatch('inputLetter', { letter });
};

const stateBgColor = {
  strike: 'bg-[#6aaa64] text-white',
  ball: 'bg-[#c9b458] text-white',
  out: 'bg-[#787c7e] text-white',
  none: 'bg-[#ffffff]',
};
</script>
<template>
  <div
    v-for="({ letter, state }, index2) in keyboardRow"
    :ref="element => (keyRef[index2] = element as Element)"
    :key="index2"
    class="font-bold text-sm uppercase mr-2 border-solid border-gray-500 h-16 border-2 rounded-md flex justify-center items-center cursor-pointer"
    :class="[letter.length > 1 ? 'w-[67px]' : 'w-[45px]', stateBgColor[state]]"
    @click="clickKey(index2, letter)"
    @animationend="clickKey(index2)"
  >
    <template v-if="letter === 'backspace'">
      <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 0 24 24" width="24">
        <path
          fill="#000000"
          d="M22 3H7c-.69 0-1.23.35-1.59.88L0 12l5.41 8.11c.36.53.9.89 1.59.89h15c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm0 16H7.07L2.4 12l4.66-7H22v14zm-11.59-2L14 13.41 17.59 17 19 15.59 15.41 12 19 8.41 17.59 7 14 10.59 10.41 7 9 8.41 12.59 12 9 15.59z"
        ></path>
      </svg>
    </template>
    <template v-else>
      {{ letter }}
    </template>
  </div>
</template>
