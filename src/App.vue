<template>
  <arona_spinePlayer
    skelUrl="/spine_assets/arona_spr.skel"
    atlasUrl="/spine_assets/arona_spr.atlas"
    animation="Idle_01"
    :backgroundColor="'#00000000'"
    :alpha="true"
    :premultipliedAlpha="true"
    :showControls="true"
    :zIndex="999"
  ></arona_spinePlayer>
</template>
<script setup lang="ts">
// @ts-ignore
import arona_spinePlayer from './components/arona_spinePlayer.vue'
</script>
<style scoped lang=""></style>
