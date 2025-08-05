<template>
  <div class="square"></div>
</template>

<script setup></script>

<style scoped lang="scss">
@keyframes opacityChange {
  to {
    --opacity: 1;
  }
}

@property --angle {
  syntax: "<angle>";
  initial-value: 0deg;
  inherits: false;
}
@keyframes rotate {
  from {
    --angle: 0deg;
  }
  to {
    --angle: 180deg;
  }
}
.square {
  width: 100%;
  height: 100%;
  border: 0.02rem solid transparent;
  border-radius: 0.1rem;
  /* Paint an image in the border */
  background-image: 
  linear-gradient(#000, #000),
  conic-gradient(
    from var(--angle),
    #fff 0deg 90deg,
    #787878 90deg 180deg,
    #fff 180deg 270deg,
    #787878 270deg 360deg
  );
  
  background-origin: border-box;
  background-clip: padding-box, border-box;
  animation: rotate 0.9s ease infinite;

  &.inactive {
    animation: none;
    border-color: #787878;
    border-image: unset;
  }
}
</style>
