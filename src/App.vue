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

const indexSet = ref(new Set());
function random() {
  indexSet.value = new Set();
  const square = Math.pow(count.value, 2);
  while (indexSet.value.size < count.value) {
    indexSet.value.add(Math.floor(Math.random() * square));
  }
}

function handleClick(size, mode) {
  setGridSize(size);
  setMode(mode);
  if (mode === modeTypes.random) {
    random();
  }
}
</script>

<template>
  <button type="button" @click="handleClick(1, modeTypes.all)">1 x 1</button>
  <button type="button" @click="handleClick(3, modeTypes.all)">3 x 3</button>
  <button type="button" @click="handleClick(5, modeTypes.all)">5 x 5</button>
  <button type="button" @click="handleClick(10, modeTypes.all)">10 x 10</button>
  <button type="button" @click="handleClick(count, modeTypes.all)">All</button>
  <button type="button" @click="handleClick(count, modeTypes.random)">Random</button>
  <section class="container">
    <Square v-for="(_i, index) in square" :key="Math.random()" :class="mode === modeTypes.random ? indexSet.has(index) ? 'active' : 'inactive' : ''" />
  </section>
</template>

<style lang="scss">
.container {
  width: 20rem;
  height: 20rem;
  margin: 0 auto;
  display: grid;
  justify-content: center;
  align-content: center;
  grid-gap: 0.5rem;
  grid-template-columns: repeat(v-bind(count), 1fr);
  grid-template-rows: repeat(v-bind(count), 1fr);
}
</style>
