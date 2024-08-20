<script setup>
import { ref } from "vue";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head, Link, router } from "@inertiajs/vue3";

const props = defineProps(["games"]);
const games = ref(props.games.data);

Echo.private("lobby").listen("GameJoined", (event) => {
  games.value = games.value.filter((game) => game.id !== event.game.id);

  if (games.value.lenght < 5) {
    router.reload({
      only: ["games"],
      onSuccess: () => (games.value = props.games.data),
    });
  }
});
</script>

<template>
  <Head title="Dashboard" />

  <AuthenticatedLayout class="text-black dark:text-white">
    <template #header>
      <h2
        class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight"
      >
        Dashboard
      </h2>
    </template>

    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div
          class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg py-4 px-6"
        >
          <Link
            :href="route('games.store')"
            method="post"
            as="button"
            class="inline-flex items-center px-4 py-2 bg-gray-800 dark:bg-gray-200 border border-transparent rounded-md font-semibold text-xs text-white dark:text-gray-800 uppercase tracking-widest hover:bg-gray-700 dark:hover:bg-white focus:bg-gray-700 dark:focus:bg-white active:bg-gray-900 dark:active:bg-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 dark:focus:ring-offset-gray-800 transition ease-in-out duration-150"
          >
            Create Game
          </Link>

          <ul class="mt-10 divide-y">
            <li
              v-for="game in games"
              :key="game.id"
              class="px-2 py-1.5 flex items-center justify-between"
            >
              <span>{{ game.player_one.name }}</span>
              <Link
                :href="route('games.join', game.id)"
                method="post"
                as="button"
                class="px-3 py-2 rounded transition-colors hover:bg-gray-100 dark:hover:bg-gray-700"
              >
                Join Game
              </Link>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </AuthenticatedLayout>
</template>
