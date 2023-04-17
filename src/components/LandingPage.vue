<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";
import AddPlayers from "../components/AddPlayer.vue"
import TicTacToe from "../components/TicTacToe.vue";

let show = ref(true);
const player1 = ref();
const player2 = ref();

// const players = ref<Player[]>(JSON.parse(localStorage.getItem("players") || "[]"));
// function saveToLs(players: Player[]) {
//     localStorage.setItem("players", JSON.stringify(players));
// }

const addPlayers = (playerX: string, playerO: string) => {
    player1.value = (new Player(playerX, "x", 0, false));
    player2.value = (new Player(playerO, "o", 0, false));
    // saveToLs(players.value);
    show.value = false;

}

</script>

<template>
    <AddPlayers @add-players="addPlayers" v-if="show" />
    <TicTacToe v-else :player1="player1" :player2="player2">
    </TicTacToe>
</template>


<script scoped></script>