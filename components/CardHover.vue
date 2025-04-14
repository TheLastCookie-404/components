<template>
  <div
    class="relative overflow-hidden base-block after:transition after:duration-300"
    :class="isMouseLeave ? 'after:opacity-0' : 'after:opacity-100'"
    :style="`padding: ${borderWidth}`"
    @mousemove="handleMouse"
    @mouseleave="isMouseLeave = true">
    <div class="size-full content-container">
      <!-- 
        Flex fixes issue with margin-top when 
        it applied to child via <slot /> 
      -->
      <div class="flex">
        <slot />
      </div>
    </div>
  </div>
</template>

<script setup>
  const mouseX = ref(0);
  const mouseY = ref(0);
  const isMouseLeave = ref(true);

  defineProps({
    /**
     * @lightColor Color of lighting effect (sircle that forms hover outline)
     */
    lightColor: {
      type: String,
      required: false,
      default: "#fff",
    },
    /**
     * @lightBlur Blur factor of lighting effect
     */
    lightBlur: {
      type: String,
      required: false,
      default: "40px",
    },
    /**
     * @lightSize Size of lighting effect
     */
    lightSize: {
      type: String,
      required: false,
      default: "100%",
    },
    /**
     * @baseColor Color of base (block that forms default outline)
     */
    baseColor: {
      type: String,
      required: false,
      default: "#15191e",
    },
    /**
     * @containerColor Color of container for slot
     */
    containerColor: {
      type: String,
      required: false,
      default: "#191e24f3",
    },
    /**
     * @containerColor Border width (difference between base and container sizes)
     */
    borderWidth: {
      type: String,
      required: false,
      default: "2px",
    },
    /**
     * @containerColor Radius of corners
     */
    borderRadius: {
      type: String,
      required: false,
      default: "0",
      validator: (value) => {
        return /^\d+(\.\d+)?(px|rem|em|%)$/.test(value);
      },
    },
  });

  function handleMouse(event) {
    isMouseLeave.value = false;
    mouseX.value = event.offsetX;
    mouseY.value = event.offsetY;
  }
</script>

<style lang="css" scoped>
  .base-block {
    border-radius: v-bind(borderRadius);
  }

  .base-block::before {
    position: absolute;
    top: 0;
    left: 0;
    z-index: -2;
    width: 100%;
    height: 100%;
    box-sizing: content-box;
    background: v-bind(baseColor);
    content: ""; /* ::before can`t be visible without content */
  }

  .base-block::after {
    position: absolute;
    top: v-bind("`${mouseY}px`");
    left: v-bind("`${mouseX}px`");
    z-index: -1;
    width: 100%;
    aspect-ratio: 1/1;
    transform: translate(calc((100% / 2) * -1), calc((100% / 2) * -1));
    border-radius: calc(infinity * 1px);
    background: v-bind(lightColor);
    filter: blur(v-bind(lightBlur));
    content: ""; /* ::after can`t be visible without content */
  }

  .content-container {
    background: v-bind(containerColor);
    border-radius: calc(v-bind(borderRadius) - v-bind(borderWidth));
  }
</style>
