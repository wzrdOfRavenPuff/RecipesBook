<template>
  <input
    type="text"
    :id="id"
    class="
      bg-gray-50
      border border-gray-300
      text-gray-900 text-sm
      rounded-lg
      focus:ring-blue-500 focus:border-blue-500
      block
      w-full
      p-2.5
      dark:bg-gray-700
      dark:border-gray-600
      dark:placeholder-gray-400
      dark:text-white
      dark:focus:ring-blue-500
      dark:focus:border-blue-500
    "
    :class="$attrs.class"
    :placeholder="$attrs.placeholder"
    v-model="value"
    required
  />
</template>
<script setup>
const { id, modelValue, label } = defineProps(['id', 'modelValue', 'label']);
const emit = defineEmits(['update:modelValue']);
var reloadTimeout;
let mValue = ref(modelValue);

const value = computed({
  get() {
    return modelValue;
  },
  set(mvalue) {
    clearTimeout(reloadTimeout);
    reloadTimeout = setTimeout(() => {
      emit('update:modelValue', mvalue);
    }, 300);
  },
});
</script>
