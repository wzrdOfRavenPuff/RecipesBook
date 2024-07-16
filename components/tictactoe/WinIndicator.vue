<template>
  <div>
    <div class="cont-ind" v-show="!isTie">
      <div class="mark-x">
        <div v-show="turnNum % 2 === 0">X</div>
      </div>
      <div class="mark-o">
        <div v-show="turnNum % 2 === 1">O</div>
      </div>
      <div>WINS</div>
    </div>
    <div class="cont-ind" v-show="isTie">
      <div class="tie">TIE</div>
    </div>
  </div>
</template>

<script setup>
const { turnNum, isTie } = defineProps({ turnNum: Number, isTie: Boolean });
</script>
<style scoped>
.cont-ind {
  font-size: 1.5rem;
  display: flex;
  align-items: center;
  margin-bottom: 0.5em;
}
.tie {
  margin: 0.25em 0;
}
.mark-x {
  font-size: 2rem;
  margin-right: 0.5em;
  color: var(--clr-acc-x);
}
.mark-o {
  font-size: 2rem;
  margin-right: 0.5em;
  color: var(--clr-acc-o);
}
</style>
