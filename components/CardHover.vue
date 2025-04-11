<template>
  <div
    class="relative box-content overflow-hidden base-block"
    :style="`padding: ${borderWidth}`"
    @mousemove="handleMouse"
    @mouseleave="isMouseLeave = true">
    <div class="size-full relative z-10 content-container">
      <slot />
    </div>
    <div class="w-full aspect-square mouse-pos z-0 pointer-events-none">
      <div class="h-full -translate-x-1/2 -translate-y-1/2">
        <div
          class="h-full transition duration-300"
          :class="isMouseLeave ? 'opacity-0' : 'opacity-100'">
          <div class="h-full rounded-full blur-2xl light-sircle" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const mouseX = ref(0);
const mouseY = ref(0);
const isMouseLeave = ref(true);

defineProps({
  lightColor: {
    type: String,
    required: false,
    default: "#fff",
  },
  lightBlur: {
    type: String,
    required: false,
    default: "40px",
  },
  baseColor: {
    type: String,
    required: false,
    default: "#15191e",
  },
  containerColor: {
    type: String,
    required: false,
    default: "#191e24f8",
  },
  borderWidth: {
    type: String,
    required: false,
    default: "2px",
  },
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
  background: v-bind(baseColor);
  border-radius: v-bind(borderRadius);
}

.light-sircle {
  background: v-bind(lightColor);
  filter: blur(v-bind(lightBlur));
}

.content-container {
  background: v-bind(containerColor);
  border-radius: calc(v-bind(borderRadius) - v-bind(borderWidth));
}

.mouse-pos {
  position: absolute;
  top: v-bind("`${mouseY}px`");
  left: v-bind("`${mouseX}px`");
}
</style>
