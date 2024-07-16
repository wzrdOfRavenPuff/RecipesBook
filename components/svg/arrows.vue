<script setup>
const { arrows } = defineProps(['arrows']);
</script>

<template>
  <svg
    height="100%"
    width="100%"
    viewBox="0 0 100 100"
    preserveAspectRatio="none"
    class="w-full h-full stroke-2 stroke-red-700 fill-blue-500"
    :class="$attrs.class"
    v-if="arrows"
  >
    <polygon v-if="arrows.bottomRight" points="100,100 89,96 96,89" />
    <polygon v-if="arrows.bottomLeft" points="0,100 11,96 4,89" />
    <polygon v-if="arrows.topRight" points="100,0 89,4 96,11" />
    <polygon v-if="arrows.topLeft" points="0,0 11,4 4,11" />
    <polygon v-if="arrows.right" points="100,50 90,45 90,55" />
    <polygon v-if="arrows.bottom" points="50,100 45,90 55,90" />
    <polygon v-if="arrows.top" points="50,0 45,10 55,10" />
    <polygon v-if="arrows.left" points="0,50 10,45 10,55" />
  </svg>
</template>

<style scoped></style>
