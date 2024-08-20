<script setup>
import { ref } from "vue";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { computed } from "vue";

const props = defineProps(["game"]);

const boardState = ref([0, 0, 0, 0, 0, 0, 0, 0, 0]);

const xTurn = computed(
  () => boardState.value.reduce((carry, value) => carry + value, 0) === 0,
);

const fillSquare = (index) => {
  boardState.value[index] = xTurn.value ? -1 : 1;
};
</script>

<template>
  <AuthenticatedLayout class="text-black dark:text-white">
    <div class="">The Game Goes Here</div>

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
  </AuthenticatedLayout>
</template>
