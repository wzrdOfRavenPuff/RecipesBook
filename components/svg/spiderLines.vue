<script setup>
const { lines } = defineProps(['lines']);
</script>

<template>
  <svg
    height="100%"
    width="100%"
    viewBox="0 0 100 100"
    preserveAspectRatio="none"
    class="w-full h-full stroke-2 stroke-red-700"
    :class="$attrs.class"
    v-if="lines"
  >
    <line v-if="lines.topLeft" x1="0" y1="0" x2="50" y2="50" />
    <line v-if="lines.bottomRight" x1="50" y1="50" x2="100" y2="100" />
    <line v-if="lines.topRight" x1="50" y1="50" x2="100" y2="0" />
    <line v-if="lines.bottomLeft" x1="50" y1="50" x2="0" y2="100" />
    <line v-if="lines.left" x1="0" y1="50" x2="50" y2="50" />
    <line v-if="lines.right" x1="50" y1="50" x2="100" y2="50" />
    <line v-if="lines.top" x1="50" y1="0" x2="50" y2="50" />
    <line v-if="lines.bottom" x1="50" y1="50" x2="50" y2="100" />
  </svg>
</template>

<style scoped></style>
