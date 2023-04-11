<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";
import AddPlayers from "../components/AddPlayer.vue"

let show = true;
const players = ref<Player[]>(JSON.parse(localStorage.getItem("players") || "[]"));

function saveToLs(players: Player[]) {
    localStorage.setItem("players", JSON.stringify(players));
}

const addPlayers = (player1: string, player2: string) => {
    players.value.push(new Player(player1, "x"));
    players.value.push(new Player(player2, "o"));
    saveToLs(players.value);

}

</script>


<template>
    <AddPlayers @add-players="addPlayers"></AddPlayers>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</template>

<style scoped></style>