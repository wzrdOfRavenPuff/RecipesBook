<template>
  <h1>Welcome to the game mastermind</h1>
  <div class="bg-orange-100 rounded border border-orange-300 p-2">
    Correct answer :
    <span
      class="border rounded inline-flex hover:text-black px-2"
      :class="
        isEnded
          ? 'bg-green-200 border-green-200 text-black'
          : 'bg-red-200 text-red-200 border-red-200'
      "
    >
      {{ correctAnswer }}
      <!-- isEnded: {{ isEnded }} -->
    </span>

    <div class="m-1">
      <div
        class="border rounded inline-flex"
        :class="fullGuess ? 'bg-blue-100 border-blue-700' : 'border-gray-700'"
      >
        <button
          v-for="num in guess"
          class="bg-gray-300 font-bold normalButton"
          :class="
            isEnded
              ? 'bg-green-200 border border-green-500'
              : fullGuess
              ? 'bg-blue-400'
              : ''
          "
          disabled
        >
          {{ num }}
        </button>
      </div>
    </div>
    <div class="m-1">
      <div
        class="inline m-0 p-0"
        v-for="(option, index) in options"
        :key="option.value"
      >
        <br v-if="index % 5 == 0" />
        <button
          class="text-gray-800 font-bold normalButton"
          :class="
            isEnded
              ? 'bg-gray-300'
              : option.selected
              ? isEnded
                ? 'bg-green-300'
                : 'bg-green-300 hover:bg-green-400'
              : !fullGuess
              ? 'bg-blue-300 hover:bg-blue-400'
              : 'bg-gray-300'
          "
          :disabled="(fullGuess && !option.selected) || isEnded"
          @click="selectOption(option)"
        >
          {{ option.value }}
        </button>
      </div>
    </div>
    <button
      @click="guessThisNumber()"
      class="btn"
      :disabled="!fullGuess || isEnded"
    >
      Guess
    </button>
    <button @click="reset()" class="btn" :disabled="!isEnded">Reset</button>
  </div>

  guesses:
  <div v-for="item in guesses">
    <!-- {{ item }} -->
    <div class="border border-gray-700 rounded p-1 m-1 inline">
      <div class="inline mr-8">{{ item.attempt }}</div>
      <div class="inline mr-8">
        <!-- class="border border-gray-700 rounded inline mr-8"> -->
        <button
          v-for="num in item.guess"
          @click="toggleCorrectness(num)"
          class="bg-gray-300 font-bold border normalButton"
          :class="
            item.numberInRightPlace == nr
              ? 'bg-green-200 border-green-500'
              : num.rightPlace
              ? 'bg-green-200 border-green-500'
              : num.wrongPlace
              ? 'bg-yellow-200 border-yellow-500'
              : num.notUsed
              ? 'bg-red-200 border-red-500'
              : ''
          "
        >
          {{ num.number }}
        </button>
      </div>
      <button
        disabled
        class="bg-green-200 font-bold border border-green-500 normalButton"
      >
        {{ item.numberInRightPlace }}
      </button>
      <button
        disabled
        class="bg-yellow-200 font-bold border border-yellow-500 normalButton"
      >
        {{ item.numberInWrongPlace }}
      </button>
    </div>
  </div>
</template>
<script setup>
const nr = 4;
const options = ref([
  { value: 1, selected: false },
  { value: 2, selected: false },
  { value: 3, selected: false },
  { value: 4, selected: false },
  { value: 5, selected: false },
  { value: 6, selected: false },
  { value: 7, selected: false },
  { value: 8, selected: false },
  { value: 9, selected: false },
  { value: 0, selected: false },
]);
let guess = ref(Array(nr).fill(null));
let fullGuess = ref(guess.value.indexOf(null) == -1);
const correctAnswer = ref(getNewCorrectAnswer());
let guesses = ref(Array(0));
let isEnded = ref(false);

function guessThisNumber() {
  if (isEnded.value == false && fullGuess.value == true) {
    let numbersRightPlace = 0;
    let numbersWrongPlace = 0;

    for (let i = 0; i < guess.value.length; i++) {
      if (guess.value[i] === correctAnswer.value[i]) {
        numbersRightPlace++;
      } else if (correctAnswer.value.includes(guess.value[i])) {
        numbersWrongPlace++;
      } else {
        //not a right number
      }
    }

    guesses.value.push(createGuess(numbersRightPlace, numbersWrongPlace));
    if (numbersRightPlace == nr) {
      isEnded.value = true;
      deselectOptions(false);
      toggleCorrectnessAutoAllNumbers();
    } else {
      deselectOptions(true);
    }
  }
}
function createGuess(numberInRightPlace, numberInWrongPlace) {
  let answer = guess.value.map((x) => {
    return { number: x, rightPlace: false, wrongPlace: false, notUsed: false };
  });
  let attempt = guesses.value.length + 1;

  return {
    guess: answer,
    numberInRightPlace: numberInRightPlace,
    numberInWrongPlace: numberInWrongPlace,
    attempt: attempt,
  };
}
function deselectOptions(deselectGuess) {
  if (deselectGuess == true) {
    guess.value = guess.value.fill(null);
  }
  fullGuess.value = false;
  options.value.forEach((element) => (element.selected = false));
}

function selectOption(option) {
  option.selected = !option.selected;
  if (option.selected) {
    const index = guess.value.indexOf(null);
    guess.value[index] = option.value;
  } else {
    const index = guess.value.indexOf(option.value);
    guess.value[index] = null;
  }
  fullGuess.value = guess.value.indexOf(null) == -1;
}

function reset() {
  deselectOptions(true);
  isEnded.value = false;
  correctAnswer.value = getNewCorrectAnswer();
  guesses.value = Array(0);
}

function getNewCorrectAnswer() {
  let randomArray = Array();

  for (let i = 0; i < nr; i++) {
    let nrFound = false;
    while (!nrFound) {
      const rand = getRandomInt(10);
      if (randomArray.length == 0 || randomArray.indexOf(rand) == -1) {
        randomArray.push(rand);
        nrFound = true;
      }
      if (randomArray.length > 3) {
        nrFound = true;
      }
    }
  }

  return randomArray;
}

function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}

function toggleCorrectness(num) {
  if (num.rightPlace) {
    num.rightPlace = false;
    num.wrongPlace = true;
  } else if (num.wrongPlace) {
    num.wrongPlace = false;
    num.notUsed = true;
  } else if (num.notUsed) {
    num.notUsed = false;
  } else {
    num.rightPlace = true;
  }
}

function toggleCorrectnessAutoAllNumbers() {
  console.log(guesses);
  for (let i = 0; i < guesses.value.length; i++) {
    const item = guesses.value[i];
    console.log(item);
    for (let j = 0; j < item.guess.length; j++) {
      console.log(item.guess[j]);
      if (item.guess[j].number === correctAnswer.value[j]) {
        item.guess[j].rightPlace = true;
        item.guess[j].wrongPlace = false;
        item.guess[j].notUsed = false;
      } else if (correctAnswer.value.includes(item.guess[j].number)) {
        item.guess[j].rightPlace = false;
        item.guess[j].wrongPlace = true;
        item.guess[j].notUsed = false;
      } else {
        item.guess[j].rightPlace = false;
        item.guess[j].wrongPlace = false;
        item.guess[j].notUsed = true;
      }
    }
  }
}
</script>
<style scoped>
.normalButton {
  /*@apply py-2 px-4 rounded m-1 w-12 h-12;*/
  @apply rounded m-1 w-12 h-12 items-center justify-center;
  /* @apply inline-flex items-center justify-center; */
}
</style>
