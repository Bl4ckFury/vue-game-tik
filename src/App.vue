<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const gameWinner = (board) => {
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
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
};

const winner = computed(() => gameWinner(board.value.flat()));
const makeMove = (x, o) => {
  if (winner.value) return;
  if (board.value[x][o] !== "") return;
  board.value[x][o] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};

const resetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<template>
  <div
    class="flex flex-row item-center justify-center pt-8 text-center dark:bg-gray-900 dark:text-white min-h-screen"
  >
    <div
      class="flex item-center justify-center pt-8 text-center dark:bg-gray-900 dark:text-white min-h-screen"
    >
      <!-- <img class="img-first" src="./assets/img/minato.png" alt="minato" /> -->
      <span class="text-pink-400 text-5xl">
        {{ firstPlayer }}
      </span>
    </div>
    <main
      class="pt-8 text-center dark:bg-gray-900 dark:text-white min-h-screen"
    >
      <h1 class="mb-8 text-6xl font-bold uppercase">Tik-Tak game</h1>
      <h3
        :class="`text-3xl mb-10 ${
          cell === 'X' ? 'text-pink-500' : 'text-blue-400'
        }`"
      >
        Игрок: {{ player }} - ваш ход!
      </h3>
      <div class="flex flex-col item-center mt-10 mb-8">
        <div
          v-for="(row, x) in board"
          :key="x"
          class="flex items-center justify-center"
        >
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="makeMove(x, y)"
            :class="`border border-white w-28 h-28 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
              cell === 'X' ? 'text-pink-400' : 'text-yellow-300'
            }`"
          >
            {{ cell === "X" ? "X" : cell === "O" ? "O" : "" }}
          </div>
        </div>
      </div>
      <h2 v-show="winner" class="text-5xl font-bold mb-8">
        Игрок - {{ winner }}, победил!
      </h2>
      <button
        @click.prevent="resetGame"
        class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-400 duration-200"
      >
        Начать заново
      </button>
    </main>
    <div
      class="flex item-center justify-center pt-8 text-center dark:bg-gray-900 dark:text-white min-h-screen"
    >
      <!-- <img class="img-second" src="./assets//img/itachi.png" alt="itachi" /> -->
      <span class="text-yellow-300 text-5xl">
        {{ secondPlayer }}
      </span>
    </div>
  </div>
</template>

<style>
.img-first {
  -webkit-box-shadow: -1px 0px 43px 0px rgb(255, 221, 0);
  -moz-box-shadow: -1px 0px 43px 0px rgb(255, 221, 0);
  box-shadow: -1px 0px 43px 0px rgb(255, 221, 0);
}

.img-second {
  -webkit-box-shadow: -1px 0px 43px 0px rgba(172, 0, 255, 0.85);
  -moz-box-shadow: -1px 0px 43px 0px rgba(172, 0, 255, 0.85);
  box-shadow: -1px 0px 43px 0px rgba(172, 0, 255, 0.85);
}
</style>
