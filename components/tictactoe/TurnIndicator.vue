<template>
  <div class="turn-ind">
    <div class="mark-x" :class="{ active: turnNum % 2 === 0 }">
      <div>X</div>
    </div>
    <div class="text">TURN</div>
    <div class="mark-o" :class="{ active: turnNum % 2 === 1 }">
      <div>O</div>
    </div>
  </div>
</template>

<script setup>
const { turnNum } = defineProps({ turnNum: Number });
</script>
<style scoped>
.cont-ind {
  font-size: 1.5rem;
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}
.cont-slot {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  width: fit-content;
  margin-bottom: 0.5em;
}
.turn-ind {
  font-size: 1.5rem;
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}
.text {
  margin: 0 1em;
}
.mark-x,
.mark-o {
  font-size: 2rem;
  color: var(--clr-faded);
  background-color: none;
  border: 0;
  border-radius: 0.2em;
}
.mark-x.active {
  color: var(--clr-acc-x);
}
.mark-o.active {
  color: var(--clr-acc-o);
}
</style>
