<script setup lang="ts">
import { ref } from "vue";

const x = ref("");
const o = ref("");

defineEmits(["addPlayers"]);

</script>

<template>
    <form>
        <label for="x">Player1</label>
        <input v-model="x">
        <label for="o">Player2</label>
        <input v-model="o">
        <button @click="$emit('addPlayers', x, o)">Save</button>
    </form>
</template>

<style scoped></style>