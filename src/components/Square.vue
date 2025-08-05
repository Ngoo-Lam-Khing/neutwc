<template>
  <div class="square"></div>
</template>
<style scoped lang="scss">
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
  border: 0.01rem solid transparent;
  border-radius: 0.02rem;
  /* Paint an image in the border */
  background-image: conic-gradient(
    from var(--angle),
    #fff 0deg 90deg,
    #787878 90deg 180deg,
    #fff 180deg 270deg,
    #787878 270deg 360deg
  );
  background-origin: border-box;
  background-clip: padding-box, border-box;

  position: relative;

  &::after {
    content: "";
    display: block;
    width: 95%;
    height: 95%;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    border-radius: 0.02rem;
    background-color: #000;
  }

  &.active {
    animation: rotate 0.9s ease infinite;
  }
  &.inactive {
    animation: none;
    background-image: conic-gradient(
      from var(--angle),
      #787878 0deg 90deg,
      #787878 90deg 180deg,
      #787878 180deg 270deg,
      #787878 270deg 360deg
    );
  }

  &--heart {
    clip-path: shape(
      from 50% 91%,
      line to 90% 50%,
      arc to 50% 9% of 1%,
      arc to 10% 50% of 1%
    );
    &::after {
      clip-path: shape(
        from 50% 91%,
        line to 90% 50%,
        arc to 50% 9% of 1%,
        arc to 10% 50% of 1%
      );
    }
  }
}
</style>
