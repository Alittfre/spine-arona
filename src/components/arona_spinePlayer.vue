<template>
  <div ref="playerContainer" :style="containerStyle"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { spine } from './spine-player.js'
// Props for dynamic configuration
const props = defineProps({
  skelUrl: {
    type: String,
    required: true,
  },
  atlasUrl: {
    type: String,
    required: true,
  },
  animation: {
    type: String,
    default: '',
  },
  width: {
    type: String,
    default: '100%',
  },
  height: {
    type: String,
    default: '100%',
  },
  backgroundColor: {
    type: String,
    default: '#00000000',
  },
  alpha: {
    type: Boolean,
    default: true,
  },
  premultipliedAlpha: {
    type: Boolean,
    default: true,
  },
  showControls: {
    type: Boolean,
    default: true,
  },
  zIndex: {
    type: Number,
    default: 999,
  },
})

// Define a ref for the container
const playerContainer = ref(null)

// Define container style
const containerStyle = {
  position: 'fixed',
  zIndex: props.zIndex,
  width: props.width,
  height: props.height,
}

// Import and configure the SpinePlayer after the component mounts
onMounted(() => {
  new spine.SpinePlayer(playerContainer.value, {
    skelUrl: props.skelUrl,
    atlasUrl: props.atlasUrl,
    animation: props.animation,
    premultipliedAlpha: props.premultipliedAlpha,
    backgroundColor: props.backgroundColor,
    alpha: props.alpha,
    viewport: props.viewport,
    showControls: props.showControls,
  })
})
</script>

<style scoped>
/* You can add scoped styles here if needed */
/* div {
  position: absolute;
} */
</style>
