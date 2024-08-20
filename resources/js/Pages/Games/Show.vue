<script setup>
import { ref } from "vue";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { computed } from "vue";

const props = defineProps(["game"]);

const boardState = ref([0, 0, 0, 0, 0, 0, 0, 0, 0]);

const xTurn = computed(
  () => boardState.value.reduce((carry, value) => carry + value, 0) === 0,
);

const lines = [
  // rows
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  // columns
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  // diagonals
  [0, 4, 8],
  [2, 4, 6],
];

const fillSquare = (index) => {
  boardState.value[index] = xTurn.value ? -1 : 1;

  const winningLine = lines
    .map((line) =>
      line.reduce((carry, index) => carry + boardState.value[index], 0),
    )
    .find((sum) => Math.abs(sum) === 3);

  if (winningLine === -3) {
    alert("X has won");
    return;
  }

  if (winningLine === 3) {
    alert("O has won");
    return;
  }

  if (!boardState.value.includes(0)) {
    alert("Stalemate!");
  }
};
</script>

<template>
  <AuthenticatedLayout class="text-black dark:text-white">
    <main class="container mx-auto">
      <div class="mt-8 flex justify-center text-2xl uppercase font-semibold">
        The Game Goes Here
      </div>

      <menu class="mt-12 mx-auto grid grid-cols-3 gap-1.5 w-0 min-w-fit">
        <li
          v-for="(square, index) in boardState"
          class="size-40 grid place-items-center bg-gray-300 dark:bg-gray-600"
        >
          <button
            v-if="square === 0"
            @click="fillSquare(index)"
            class="bg-gray-200 dark:bg-gray-700 place-self-stretch hover:bg-gray-300 dark:hover:bg-gray-600 transition-colors"
          ></button>
          <span
            v-else
            v-text="square === -1 ? 'X' : 'O'"
            class="text-4xl font-bold"
          ></span>
        </li>
      </menu>

      <ul class="max-w-sm mx-auto mt-6 space-y-2">
        <li class="flex items-center gap-2">
          <span
            class="bg-gray-300 px-2 py-0.5 font-bold font-mono rounded dark:bg-gray-600"
          >
            X
          </span>
          <span>Test User</span>
          <span class="bg-red-500 size-2 rounded-full"></span>
        </li>
        <li class="flex items-center gap-2">
          <span
            class="bg-gray-300 px-2 py-0.5 font-bold font-mono rounded dark:bg-gray-600"
          >
            O
          </span>
          <span>Ian</span>
          <span class="bg-red-500 size-2 rounded-full"></span>
        </li>
      </ul>
    </main>
  </AuthenticatedLayout>
</template>
