<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";

const props = defineProps(["game"]);

console.log(props.game.player_one_id);
</script>

<template>
    <AuthenticatedLayout class="text-black dark:text-white">
        <div class="">The Game Goes Here</div>

        {{ game }}
    </AuthenticatedLayout>
</template>
