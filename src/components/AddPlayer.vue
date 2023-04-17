<script setup lang="ts">
import { ref } from "vue";

const x = ref("");
const o = ref("");

defineEmits(["addPlayers"]);

</script>

<template>
    <form>
        <h1>TicTacToe</h1>
        <label for="x">Player X</label>
        <input v-model="x">
        <label for="o">Player O</label>
        <input v-model="o">
        <button @click="$emit('addPlayers', x, o)">Save</button>
    </form>
</template>

<style scoped></style>