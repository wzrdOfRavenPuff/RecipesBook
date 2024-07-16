<template>
  <h1>Clever 1</h1>
  <div>
    <div class="bg-red-200 grid grid-cols-7">
      <div class="bg-green-200 m-1">
        <div class="bg-orange-200 m-1">
          <div class="bg-yellow-200 m-1">die 1</div>
          <div class="bg-yellow-200 m-1">die 2</div>
          <div class="bg-yellow-200 m-1">die 3</div>
        </div>
      </div>
      <div class="col-span-6 bg-green-200 m-1">
        <div class="bg-orange-200 m-1">
          <div class="grid grid-cols-6 bg-yellow-200 m-1">
            <div class="border border-yellow-600 m-1">
              <div class="bg-blue-200 m-1">1</div>
              <div class="bg-blue-200 m-1">1</div>
            </div>
            <div class="border border-yellow-600 m-1">
              <div class="bg-blue-200 m-1">2</div>
              <div class="bg-blue-200 m-1">2</div>
            </div>
            <div class="border border-yellow-600 m-1">
              <div class="bg-blue-200 m-1">3</div>
              <div class="bg-blue-200 m-1">3</div>
            </div>
            <div class="border border-yellow-600 m-1">
              <div class="bg-blue-200 m-1">4</div>
              <div class="bg-blue-200 m-1">4</div>
            </div>
            <div class="border border-yellow-600 m-1">
              <div class="bg-blue-200 m-1">5</div>
              <div class="bg-blue-200 m-1">5</div>
            </div>
            <div class="border border-yellow-600 m-1">
              <div class="bg-blue-200 m-1">6</div>
              <div class="bg-blue-200 m-1">6</div>
            </div>
          </div>
          <div class="grid grid-cols-8 bg-yellow-200 m-1">
            <div class="border border-yellow-600 m-1">reroll</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
          </div>
          <div class="grid grid-cols-8 bg-yellow-200 m-1">
            <div class="border border-yellow-600 m-1">plusses</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
            <div class="border border-yellow-600 m-1">check</div>
          </div>
        </div>
      </div>
    </div>
    <div>
      <!-- <pre>{{ pageLayout.blue }}</pre> -->
    </div>
    <div class="grid grid-cols-2">
      <div class="bg-yellow-200 col-span-2 sm:col-span-1">
        <div>yellow</div>
      </div>
      <div class="bg-blue-200 col-span-2 sm:col-span-1">
        <div class="">points</div>
        <div class="grid grid-cols-5">
          <div class="inline m-0 p-0" v-for="plb in blocks.blue.pageLayout">
            <button
              class="normalButton"
              :class="{
                'text-gray-200': plb.special == 'black',
                'text-gray-800': plb.special != 'black',
                'bg-white': !plb.special,
                'bg-green-200 border border-green-500': plb.special == 'green',
                'bg-purple-200 border border-purple-500':
                  plb.special == 'purple',
                'bg-yellow-200 border border-yellow-500':
                  plb.special == 'yellow',
                'bg-orange-200 border border-orange-500':
                  plb.special == 'orange',
                'bg-blue-200 border border-blue-500': plb.special == 'blue',
                'bg-red-200 border border-red-500': plb.special == 'fox',
                'bg-black border border-gray-500': plb.special == 'black',
              }"
            >
              {{
                plb.special
                  ? plb.options
                    ? plb.options.join('/')
                    : 'fox'
                  : plb.value
              }}
            </button>
          </div>
        </div>
      </div>
      <div class="col-span-2 bg-green-200">green</div>
      <div class="col-span-2 bg-orange-200">orange</div>
      <div class="col-span-2 bg-purple-200">purple</div>
    </div>
  </div>
</template>
<script setup>
const blocks = ref({
  blue: {
    pageLayout: [
      {
        value: 2,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 5,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 7,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 10,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: true, bottom: false },
      },
      {
        value: null,
        special: 'fox',
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 3,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 6,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 8,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 11,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: true, bottom: false },
      },
      {
        value: null,
        special: 'green',
        options: ['x'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 4,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: false },
        arrows: { right: false, bottom: true },
      },
      {
        value: 7,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: true },
      },
      {
        value: 9,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: true },
      },
      {
        value: 12,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: true, bottom: true },
      },
      {
        value: null,
        special: 'purple',
        options: ['6'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'orange',
        options: ['1', '6'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'blue',
        options: ['x'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'yellow',
        options: ['x'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'black',
        options: ['+1'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
    ],
  },
});
</script>
<style scoped>
.normalButton {
  @apply py-2 px-4 rounded m-1 w-12 h-12;
}
</style>
