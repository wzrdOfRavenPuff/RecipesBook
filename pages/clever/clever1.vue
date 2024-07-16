<template>
  <h1>Clever 1</h1>
  <div>
    <div
      class="bg-red-900 text-white p-2 m-2 rounded-xl"
      @click="showTodo = !showTodo"
    >
      TODO:
      <ul
        class="list-disc list-inside"
        v-if="showTodo"
        @click="showTodo = !showTodo"
      >
        <li>add "new round" button to reset dice</li>
        <li>add steps for round (to track 3 steps per round)</li>
        <li>
          only possible to select 1 die per step (disable "roll"-button untill
          selected?)
        </li>
        <li>complete possibleOptions</li>
        <li>
          if option in possibleOptions => then possible to click option in block
        </li>
        <li></li>
      </ul>
    </div>
    <!-- Plateau dice -->
    <div class="grid grid-cols-6 bg-gray-400 rounded-full">
      <div
        class="inline-flex items-center justify-center"
        v-for="die in plateauDice"
      >
        <button
          class="h-16 w-16"
          :class="{ 'bg-teal-200': die.locked }"
          @click="die.plateau = !die.plateau"
          :disabled="die.locked"
        >
          <svgDiceSix
            :number="die.number"
            class="h-full w-full"
            :class="{
              'fill-green-200 stroke-green-500': die.color == 'green',
              'fill-purple-200 stroke-purple-500': die.color == 'purple',
              'fill-yellow-200 stroke-yellow-500': die.color == 'yellow',
              'fill-orange-200 stroke-orange-500': die.color == 'orange',
              'fill-blue-200 stroke-blue-500': die.color == 'blue',
              'fill-white stroke-gray-700': die.color == 'white',
            }"
          />
        </button>
      </div>
    </div>
    <!-- Rolled dice -->
    <div class="grid grid-cols-6">
      <div class="inline-flex items-center justify-center" v-for="die in dice">
        <button
          class="h-16 w-16"
          :class="{ 'bg-teal-200': die.locked }"
          @click="selectThisDie(die)"
          :disabled="die.locked || roundEnded"
        >
          <svgDiceSix
            :number="die.number"
            class="h-full w-full"
            :class="{
              'fill-green-200 stroke-green-500': die.color == 'green',
              'fill-purple-200 stroke-purple-500': die.color == 'purple',
              'fill-yellow-200 stroke-yellow-500': die.color == 'yellow',
              'fill-orange-200 stroke-orange-500': die.color == 'orange',
              'fill-blue-200 stroke-blue-500': die.color == 'blue',
              'fill-white stroke-gray-700': die.color == 'white',
            }"
          />
        </button>
      </div>
    </div>
    <div class="inline-flex items-center justify-center">
      <button @click="rollDice" class="btn" :disabled="roundEnded">Roll</button>
      <button @click="rollDice" class="btn" :disabled="roundEnded">
        Reroll
      </button>
    </div>
    <div class="bg-red-200 grid grid-cols-7">
      <!-- selected dice -->
      <div class="bg-green-200 m-1">
        <div
          v-for="die in selectedDice"
          class="w-full inline-flex items-center justify-center"
        >
          <div class="h-16 w-16">
            <svgDiceSix
              :number="die.number"
              class="h-full w-full"
              :class="{
                'fill-green-200 stroke-green-500': die.color == 'green',
                'fill-purple-200 stroke-purple-500': die.color == 'purple',
                'fill-yellow-200 stroke-yellow-500': die.color == 'yellow',
                'fill-orange-200 stroke-orange-500': die.color == 'orange',
                'fill-blue-200 stroke-blue-500': die.color == 'blue',
                'fill-white stroke-gray-700': die.color == 'white',
              }"
            />
          </div>
        </div>
      </div>
      <!-- trackers -->
      <div class="col-span-6 bg-green-200 m-1">
        <div class="bg-orange-200 m-1">
          <!-- rounds -->
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
          <!-- rerolls -->
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
          <!-- plusses -->
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
      <div>{{ possibleOptions }}</div>
      <div>{{ selectedDice }}</div>
      <!-- <pre>{{ possibleClicks }}</pre> -->
      <div
        class="inline m-0 p-1 bg-red-200"
        v-for="possibilities in possibleClicks"
      >
        <div
          class="inline m-0 p-0 bg-teal-200"
          v-for="possible in possibilities"
        >
          <button
            class="normalButton"
            :class="{
              '!bg-gray-400': possible.selected,
              'text-gray-200':
                possible.blockToSelect == 'black' && !possible.selected,
              'text-gray-800': possible.blockToSelect != 'black',
              'bg-gray-100': !possible.blockToSelect,
              'bg-green-200 border border-green-500':
                possible.blockToSelect == 'green',
              'bg-purple-200 border border-purple-500':
                possible.blockToSelect == 'purple',
              'bg-yellow-200 border border-yellow-500':
                possible.blockToSelect == 'yellow',
              'bg-orange-200 border border-orange-500':
                possible.blockToSelect == 'orange',
              'bg-blue-200 border border-blue-500':
                possible.blockToSelect == 'blue',
              'bg-red-200 border border-red-500':
                possible.blockToSelect == 'fox',
              'bg-black border border-gray-500':
                possible.blockToSelect == 'black',
            }"
          >
            {{ possible.value }}
          </button>
        </div>
      </div>
    </div>

    <div class="grid grid-cols-2">
      <div class="bg-yellow-200 col-span-2 sm:col-span-1">
        <div class="grid grid-cols-5">
          <div
            class="relative inline-flex items-center justify-center w-full h-20 m-0 p-0"
            v-for="ply in blocks.yellow.pageLayout"
          >
            <svgSpiderLines
              v-if="ply.lines"
              :lines="ply.lines"
              class="absolute top-0 left-0 w-full h-full !stroke-1 stroke-gray-900"
            />
            <svgArrows
              v-if="ply.arrows"
              :arrows="ply.arrows"
              class="absolute top-0 left-0 w-full h-full !stroke-1 stroke-gray-900 fill-gray-900"
            />

            <button
              v-if="!ply.hide"
              class="absolute normalButton"
              :class="{
                '!bg-gray-400': ply.selected,
                'text-gray-200': ply.special == 'black' && !ply.selected,
                'text-gray-800': ply.special != 'black',
                'bg-gray-100': !ply.special,
                'bg-green-200 border border-green-500': ply.special == 'green',
                'bg-purple-200 border border-purple-500':
                  ply.special == 'purple',
                'bg-yellow-200 border border-yellow-500':
                  ply.special == 'yellow',
                'bg-orange-200 border border-orange-500':
                  ply.special == 'orange',
                'bg-blue-200 border border-blue-500': ply.special == 'blue',
                'bg-red-200 border border-red-500': ply.special == 'fox',
                'bg-black border border-gray-500': ply.special == 'black',
                points: ply.special == 'points',
                'ring ring-offset-2': ply.possible && !ply.selected,
              }"
              @click="selectThisBlock(ply)"
            >
              {{
                ply.special
                  ? ply.options
                    ? ply.options.join('/')
                    : 'fox'
                  : ply.value
              }}
            </button>
          </div>
        </div>
      </div>
      <div class="bg-blue-200 col-span-2 sm:col-span-1">
        <div class="h-20">points</div>
        <div class="grid grid-cols-5">
          <div
            class="relative inline-flex items-center justify-center w-full h-20 m-0 p-0"
            v-for="plb in blocks.blue.pageLayout"
          >
            <svgSpiderLines
              v-if="plb.lines"
              :lines="plb.lines"
              class="absolute top-0 left-0 w-full h-full !stroke-1 stroke-gray-900 fill-gray-900"
            />
            <svgArrows
              v-if="plb.arrows"
              :arrows="plb.arrows"
              class="absolute top-0 left-0 w-full h-full !stroke-1 stroke-gray-900 fill-gray-900"
            />
            <button
              v-if="!plb.hide"
              class="absolute normalButton"
              :class="{
                '!bg-gray-400': plb.selected,
                'text-gray-200': plb.special == 'black' && !plb.selected,
                'text-gray-800': plb.special != 'black',
                'bg-gray-100': !plb.special,
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
                'ring ring-offset-2': plb.possible && !plb.selected,
              }"
              @click="selectThisBlock(plb)"
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
let showTodo = ref(false);
let possibleClicks = ref(Array(0));
const dice = ref([
  {
    color: 'yellow',
    number: null,
    selected: false,
    locked: false,
    position: null,
  },
  {
    color: 'blue',
    number: null,
    selected: false,
    locked: false,
    position: null,
  },
  {
    color: 'green',
    number: null,
    selected: false,
    locked: false,
    position: null,
  },
  {
    color: 'orange',
    number: null,
    selected: false,
    locked: false,
    position: null,
  },
  {
    color: 'purple',
    number: null,
    selected: false,
    locked: false,
    position: null,
  },
  {
    color: 'white',
    number: null,
    selected: false,
    locked: false,
    position: null,
  },
]);
const blocks = ref({
  yellow: {
    pageLayout: [
      {
        //hide: true,
        value: 3,
        selected: false,
        possible: true,
        lines: {
          right: true,
          bottom: true,
          top: false,
          left: false,
          bottomRight: true,
        },
        arrows: { right: false, bottom: false },
      },
      {
        value: 6,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 5,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        selected: true,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: true, bottom: false },
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
        value: 2,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 1,
        selected: false,
        lines: {
          right: true,
          bottom: true,
          top: true,
          left: true,
          topLeft: true,
          bottomRight: true,
        },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        selected: true,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 5,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: true, bottom: false },
      },
      {
        value: null,
        special: 'orange',
        options: ['4'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 1,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        selected: true,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 2,
        selected: false,
        lines: {
          right: true,
          bottom: true,
          top: true,
          left: true,
          topLeft: true,
          bottomRight: true,
        },
        arrows: { right: false, bottom: false },
      },
      {
        value: 4,
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
        value: null,
        selected: true,
        lines: { right: true, bottom: true, top: true, left: false },
        arrows: { right: false, bottom: true },
      },
      {
        value: 3,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: true },
      },
      {
        value: 4,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: true },
      },
      {
        value: 6,
        selected: false,
        lines: {
          right: true,
          bottom: true,
          top: true,
          left: true,
          topLeft: true,
          bottomRight: true,
        },
        arrows: { right: true, bottom: true, bottomRight: true },
      },
      {
        value: null,
        special: 'fox',
        //options: ['x'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'points',
        options: ['10'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'points',
        options: ['14'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'points',
        options: ['16'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'points',
        options: ['20'],
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
  blue: {
    pageLayout: [
      {
        hide: true,
        value: null,
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 2,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 3,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 4,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: true },
        arrows: { right: true, bottom: false },
      },
      {
        value: null,
        special: 'orange',
        options: ['5'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 5,
        selected: false,
        lines: { right: true, bottom: true, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: 6,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 7,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: false },
      },
      {
        value: 8,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: true, bottom: false },
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
        value: 9,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: false },
        arrows: { right: false, bottom: true },
      },
      {
        value: 10,
        selected: false,
        lines: { right: true, bottom: true, top: true, left: true },
        arrows: { right: false, bottom: true },
      },
      {
        value: 11,
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
        special: 'fox',
        //options: ['6'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
      },
      {
        value: null,
        special: 'black',
        options: ['reroll'],
        selected: false,
        lines: { right: false, bottom: false, top: false, left: false },
        arrows: { right: false, bottom: false },
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
        value: null,
        special: 'purple',
        options: ['6'],
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

const possibleOptions = computed(() => {
  let po = {};
  for (let i = 0; i < dice.value.length; i++) {
    const die = dice.value[i];
    if (!die.locked && !die.selected && die.number) {
      po[die.color] = die.number;
    }
  }
  return po;
});

const plateauDice = computed(() => {
  let po = [];
  for (let i = 0; i < dice.value.length; i++) {
    const die = dice.value[i];
    let add = false;
    if (die.plateau && die.number) {
      add = true;
    }
    if (roundEnded.value && !die.selected) {
      add = true;
    }
    if (add) {
      po.push({ color: die.color, number: die.number });
    }
  }
  return po;
});

const selectedDice = computed(() => {
  let po = [];
  const filtered = dice.value.filter((x) => x.selected);
  if (filtered) {
    filtered.sort((a, b) => a.position - b.position);
    for (let i = 0; i < filtered.length; i++) {
      const die = filtered[i];
      if (die.selected && die.number) {
        po.push({
          color: die.color,
          number: die.number,
          position: die.position,
        });
      }
    }
  }
  return po;
});

const roundEnded = computed(() => {
  return selectedDice.value.length > 2;
});

function selectThisDie(die) {
  die.position = selectedDice.value.length + 1;
  die.selected = true;
  die.locked = true;
}

function selectThisBlock(block) {
  console.log(block);
  //if (block.possibleToSelect) {
  if (!block.selected && block.special) {
    if (block.special != 'fox' && block.special != 'points' && block.options) {
      let possibilities = [];
      block.options.forEach((option) => {
        let possible = { blockToSelect: block.special, value: option };
        possibilities.push(possible);
      });
      possibleClicks.value.push(possibilities);
    }
  }
  if (block.possibleToSelect) {
    if (block.selected && !block.value && !block.special) {
      console.debug('default block');
    } else {
      block.selected = !block.selected;
    }

    console.log(block.selected);
  }
}

function addToPosibleClicks(blockToSelect, value) {
  if (blockToSelect != 'fox') {
    possibleClicks.value.push({ blockToSelect: blockToSelect, value: value });
  }
}

function rollDice() {
  for (let i = 0; i < dice.value.length; i++) {
    if (!dice.value[i].locked) {
      const rand = getRandomInt(6);
      dice.value[i].number = rand + 1;
    }
  }
}

function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}
</script>
<style scoped>
.normalButton {
  /*@apply py-2 px-4 rounded m-1 w-12 h-12;*/
  @apply rounded m-1 w-12 h-12 items-center justify-center;
  /* @apply inline-flex items-center justify-center; */
}

.points {
  @apply border-dashed border-gray-500 border-2;
}
</style>
