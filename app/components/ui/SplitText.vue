<template>
  <span class="split-text" aria-label="text">
    <span 
      v-for="(char, index) in characters" 
      :key="index"
      class="char"
      :style="{ display: char === ' ' ? 'inline' : 'inline-block' }"
    >
      {{ char === ' ' ? '&nbsp;' : char }}
    </span>
  </span>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  text: {
    type: String,
    required: true
  }
})

const characters = computed(() => {
  return props.text.split('')
})
</script>

<style scoped>
.split-text {
  display: inline-block;
}
.char {
  position: relative;
  will-change: transform, opacity, filter;
}
</style>
