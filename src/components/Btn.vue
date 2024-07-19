<template>
  <button class="btn" :style="{ backgroundColor }" :class="{ circle: applyCircleClass }" v-bind="$attrs">
    <slot />
  </button>
</template>

<script setup>
import { computed } from 'vue';
import { useBackgroundColor, BackgroundColorProps } from './composables/backgroundColor';

const props = defineProps({
  circle: {
    default: false,
    type: Boolean,
  },

  ...BackgroundColorProps,
});

const backgroundColor = useBackgroundColor(props);

const applyCircleClass = computed(() => {
  return props.circle;
})
</script>

<style scoped>
.btn {
  color: var(--text-color);
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn:disabled {
  opacity: 80%;
}

.circle {
  border-radius: 50%;
}
</style>