<script setup>
import { computed, ref, watch } from "vue";
import Square from "./components/Square.vue";

const count = ref(1);
const square = computed(() => count.value * count.value);
function setGridSize(size) {
  count.value = size;
}

const modeTypes = {
  all: "all",
  random: "random",
};
const mode = ref(modeTypes.all);
function setMode(event) {
  mode.value = event.target.value;
}

const indexSet = ref(new Set());
function random() {
  indexSet.value = new Set();
  const square = Math.pow(count.value, 2);
  while (indexSet.value.size < count.value) {
    indexSet.value.add(Math.floor(Math.random() * square));
  }
}

watch([count, mode], ([newCount, newMode]) => {
  if (newMode === modeTypes.random && newCount > 1) {
    random();
  } else {
    indexSet.value.clear();
  }
});
</script>

<template>
  <button type="button" @click="setGridSize(1)">1 x 1</button>
  <button type="button" @click="setGridSize(3)">3 x 3</button>
  <button type="button" @click="setGridSize(5)">5 x 5</button>
  <button type="button" @click="setGridSize(10)">10 x 10</button>
  <input
    type="radio"
    name="mode"
    id="all"
    :value="modeTypes.all"
    @change="setMode"
    :checked="mode === modeTypes.all"
  />
  <label for="all">All</label>
  <input
    type="radio"
    name="mode"
    id="random"
    :value="modeTypes.random"
    @change="setMode"
    :checked="mode === modeTypes.random"
  />
  <label for="random">Random</label>
  <section class="container">
    <Square
      v-for="(_i, index) in square"
      :key="Math.random()"
      :class="
        count === 1 || mode === modeTypes.all || indexSet.has(index)
          ? 'active'
          : 'inactive'
      "
    />
  </section>
</template>

<style lang="scss">
.container {
  width: 6rem;
  height: 6rem;
  margin: 0 auto;
  display: grid;
  justify-content: center;
  align-content: center;
  grid-gap: 0.1rem;
  grid-template-columns: repeat(v-bind(count), 1fr);
  grid-template-rows: repeat(v-bind(count), 1fr);
}
</style>
