<template>
  <div class="centered">
    <h1>TIC-TAC-TOE</h1>
    <!-- <TictactoeTurnIndicator v-show="!isEnded" :turnNum="turn" /> -->
    <div v-if="!isEnded" class="m-3 text-xl">
      <div
        class="inline m-0 p-2 rounded"
        :class="turn % 2 === 0 ? 'bg-red-200' : ''"
      >
        X
      </div>
      <div class="inline m-0 p-2">TURN</div>
      <div
        class="inline m-0 p-2 rounded"
        :class="turn % 2 === 1 ? 'bg-green-200' : ''"
      >
        O
      </div>
    </div>

    <!-- <TictactoeWinIndicator v-show="isEnded" :turnNum="turn" :isTie="isTie" /> -->
    <div v-if="isEnded" class="m-3 text-xl">
      <div
        class="rounded inline m-0 p-2"
        :class="isTie ? '' : turn % 2 === 0 ? 'bg-red-200' : 'bg-green-200'"
      >
        {{ isTie ? '' : turn % 2 === 0 ? 'X' : 'O' }}
        {{ isTie ? 'TIE' : ' WINS' }}
      </div>
      <!-- <div class="rounded inline-block mx-3 p-2" v-show="isTie">TIE</div> -->
    </div>

    <div class="m-2">
      <!-- Slots are 0 indexed -->
      <!-- <TictactoeMarkSlot
        v-for="slotID in 9"
        :key="slotID - 1"
        @click="onMarkSlotClick(slotID - 1)"
        :mark="board[slotID - 1]"
      /> -->
      <div class="inline m-0 p-0" v-for="slotID in 9" :key="slotID - 1">
        <button
          @click="onMarkSlotClick(slotID - 1)"
          class="m-1 rounded border bg-gray-300 text-black w-16 h-16"
          :class="
            board[slotID - 1] === 0
              ? 'bg-red-100 border-red-300 hover:bg-red-100'
              : board[slotID - 1] === 1
              ? 'bg-green-100 border-green-300 hover:bg-green-100'
              : isEnded
              ? 'hover:bg-gray-300'
              : turn % 2 === 0
              ? 'hover:bg-red-200'
              : 'hover:bg-green-200'
          "
          :disabled="isEnded"
        >
          {{
            board[slotID - 1] === 0 ? 'X' : board[slotID - 1] === 1 ? 'O' : '-'
          }}
        </button>
        <br v-if="slotID % 3 == 0" />
      </div>
    </div>
    <button id="btn-reset" class="btn m-3" @click="resetBoard">Reset</button>
  </div>
  <!-- {{ board }} -->
</template>

<script setup>
let turn = ref(0);
let board = ref(Array(9).fill(null));
let isEnded = ref(false);
let isTie = ref(false);

function resetBoard() {
  console.log('Resetting board.');
  board.value = Array(9).fill(null);
  turn.value = 0;
  isEnded.value = false;
  isTie.value = false;
}

function onMarkSlotClick(id) {
  // Copying board so we can replace the value
  const boardCopy = board.value.slice();

  // Check if the game has ended or the slot has already been filled.
  if (isEnded.value || boardCopy[id] != null) {
    console.log(
      `isEnded: ${isEnded.value} OR board[id] != -1 ${boardCopy[id]}`
    );
    return;
  }

  // Converts the turn into the corresponding mark.
  // x = 0 | o = 1
  let mark = turn.value % 2;
  boardCopy[id] = mark;
  board.value = boardCopy;

  calculateWinner();
}

function calculateWinner() {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];

    if (
      board.value[a] != null &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      console.log(`We found a winner: ${lines[i]}`);
      isEnded.value = true;
      return;
    }
  }

  if (turn.value >= 8) {
    console.log("It's a tie.");
    isEnded.value = true;
    isTie.value = true;
    return;
  }

  if (!isEnded.value) {
    turn.value += 1;
  }

  return;
}
</script>

<style scoped></style>
