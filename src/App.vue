<script setup>
import { computed, ref, watch } from "vue";
import Square from "./components/Square.vue";

// Reactive state for grid size
const count = ref(1);
const square = computed(() => count.value * count.value);
function setGridSize(size) {
  count.value = size;
}

// Mode types for grid behavior
const modeTypes = {
  all: "all",
  random: "random",
};
const curMode = ref(modeTypes.all);
function setMode(event) {
  curMode.value = event.target.value;
}
// Set to track active squares in random mode
const indexSet = ref(new Set());
function random() {
  indexSet.value = new Set();
  const square = Math.pow(count.value, 2);
  while (indexSet.value.size < count.value) {
    indexSet.value.add(Math.floor(Math.random() * square));
  }
}
// Watch for changes in count and curMode to update indexSet
watch([count, curMode], ([newCount, newCurMode]) => {
  if (newCurMode === modeTypes.random && newCount > 1) {
    random();
  } else {
    indexSet.value.clear();
  }
});
// Shape types for square component
const shapeTypes = {
  square: "square",
  heart: "heart",
};
const curShape = ref(shapeTypes.square);
function setShape(event) {
  curShape.value = event.target.value;
}
</script>

<template>
  <button
    type="button"
    v-for="size in [1, 3, 5, 10]"
    @click="setGridSize(size)"
  >
    {{ size }} x {{ size }}
  </button>
  <label v-for="mode in modeTypes" :for="mode">
    <input
      type="radio"
      name="mode"
      :id="mode"
      :value="mode"
      @change="setMode"
      :checked="mode === curMode"
    />
    {{ mode }}
  </label>
  <label v-for="shape in shapeTypes" :for="shape">
    <input
      type="radio"
      name="shape"
      :id="shape"
      :value="shape"
      @change="setShape"
      :checked="shape === curShape"
    />
    {{ shape }}
  </label>
  <section class="container">
    <Square
      v-for="(_i, index) in square"
      :key="Math.random()"
      :class="[
        count === 1 || curMode === modeTypes.all || indexSet.has(index)
          ? 'active'
          : 'inactive',
        curShape === shapeTypes.heart ? 'square--heart' : '',
      ]"
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
