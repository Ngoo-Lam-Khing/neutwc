<template>
  <div class="square"></div>
</template>

<script setup></script>

<style scoped lang="scss">

.square {
  width: var(--font-size);
  height: var(--font-size);
  background-color: #fff;
  outline: 1px solid red;

  &.active {
    background-color: #fff;
  }

  &.inactive {
    // background-color: #0e100f;
    background-color: transparent;
  }
}
</style>
