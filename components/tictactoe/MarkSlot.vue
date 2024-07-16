<template>
  <button
    class="cont-ind"
    :class="mark === 0 ? 'mark-x-bg' : mark === 1 ? 'mark-o-bg' : ''"
  >
    <div class="mark mark-x" v-show="mark === 0">X</div>
    <div class="mark mark-o" v-show="mark === 1">O</div>
  </button>
</template>

<script setup>
const { mark } = defineProps({ mark: Number });
</script>

<style scoped>
.mark {
  font-size: 2rem;
  margin: auto;
}
.mark-x {
  /* color: var(--clr-acc-x); */
}
.mark-o {
  /* color: var(--clr-acc-o); */
}
.mark-x-bg {
  background-color: var(--clr-acc-x);
}
.mark-o-bg {
  background-color: var(--clr-acc-o);
}
button {
  color: var(--clr-bg);
  background-color: var(--clr-acc);
  cursor: pointer;
  border: 0;
  border-radius: 0.2em;
  font-size: 2rem;
  height: 3rem;
  margin: 0;
  padding: 0;
}
.cont-ind {
  font-size: 1.5rem;
  display: flex;
  width: 90%;
  height: 90%;
  min-height: 3rem;
  align-items: center;
  justify-content: middle;
  margin: auto;
  padding: 0;
}
</style>
