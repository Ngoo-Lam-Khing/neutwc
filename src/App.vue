<script setup>
import { computed, ref } from "vue";
import Square from "./components/Square.vue";

const count = ref(1);
const square = computed(() => count.value * count.value);
function setGridSize(size) {
  count.value = size;
}

const modeTypes = {
  all: "all",
  random: "random",
}
const mode = ref(modeTypes);
function setMode(newMode) {
  mode.value = newMode;
}

const fiveSquared = Math.pow(5, 2);
const indexSet = ref(new Set());
function randomPickFive() {
  indexSet.value = new Set();
  while (indexSet.value.size < 5) {
    indexSet.value.add(Math.floor(Math.random() * fiveSquared));
  }
}

const animationName = ref('move');
const animationPlayState = ref('running');
function handleAnimation() {
  if (animationPlayState.value === 'paused') {
    animationName.value = '';
    animationPlayState.value = 'running';
    setTimeout(() => {
      animationName.value = 'move';
    }, 100);
  }
}

function handleClick(size, mode) {
  setGridSize(size);
  setMode(mode);
  if (size === 5 && mode === modeTypes.random) {
    randomPickFive();
  }
  handleAnimation();
}
</script>

<template>
  <button type="button" @click="handleClick(1, modeTypes.all)">1 x 1</button>
  <button type="button" @click="handleClick(3, modeTypes.all)">3 x 3</button>
  <button type="button" @click="handleClick(5, modeTypes.all)">5 x 5</button>
  <button type="button" @click="handleClick(10, modeTypes.all)">10 x 10</button>
  <button type="button" @click="handleClick(5, modeTypes.all)">All 5 x 5 Grids</button>
  <button type="button" @click="handleClick(5, modeTypes.random)">Show Random 5 in 5 x 5 Grid</button>
  <section class="container" @animationend="animationPlayState = 'paused'" :style="{ 'animation-name': animationName, 'animation-play-state': animationPlayState }">
    <Square v-for="(i, index) in square" :key="i" :class="mode === modeTypes.random ? indexSet.has(index) ? 'active' : 'inactive' : ''" />
  </section>
</template>

<style lang="scss">
@keyframes move {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(100vw - 100%));
  }
}
.container {
  width: fit-content;
  display: grid;
  grid-template-columns: repeat(v-bind(count), var(--font-size));
  grid-template-rows: repeat(v-bind(count), var(--font-size));
  animation: 2s ease forwards;
}
</style>
