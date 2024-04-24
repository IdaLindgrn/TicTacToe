<script setup lang="ts">
import { Box } from "../models/Box";
import { onMounted, ref } from "vue";
import { Player } from "../models/Player";

let boxes = ref([new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, "")]);
let playable = true;

interface ITicTacProps {
    player1: Player;
    player2: Player;
}

function calculateWinner() {

    if ((boxes.value[0].symbolValue === "o") && (boxes.value[1].symbolValue === "o") && (boxes.value[2].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[3].symbolValue === "o") && (boxes.value[4].symbolValue === "o") && (boxes.value[5].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[6].symbolValue === "o") && (boxes.value[7].symbolValue === "o") && (boxes.value[8].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[0].symbolValue === "o") && (boxes.value[3].symbolValue === "o") && (boxes.value[6].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[1].symbolValue === "o") && (boxes.value[4].symbolValue === "o") && (boxes.value[7].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[2].symbolValue === "o") && (boxes.value[5].symbolValue === "o") && (boxes.value[8].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[0].symbolValue === "o") && (boxes.value[4].symbolValue === "o") && (boxes.value[8].symbolValue === "o")) { winner(); return; }
    if ((boxes.value[2].symbolValue === "o") && (boxes.value[4].symbolValue === "o") && (boxes.value[6].symbolValue === "o")) { winner(); return; }

    if ((boxes.value[0].symbolValue === "x") && (boxes.value[1].symbolValue === "x") && (boxes.value[2].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[3].symbolValue === "x") && (boxes.value[4].symbolValue === "x") && (boxes.value[5].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[6].symbolValue === "x") && (boxes.value[7].symbolValue === "x") && (boxes.value[8].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[0].symbolValue === "x") && (boxes.value[3].symbolValue === "x") && (boxes.value[6].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[1].symbolValue === "x") && (boxes.value[4].symbolValue === "x") && (boxes.value[7].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[2].symbolValue === "x") && (boxes.value[5].symbolValue === "x") && (boxes.value[8].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[0].symbolValue === "x") && (boxes.value[4].symbolValue === "x") && (boxes.value[8].symbolValue === "x")) { winner(); return; }
    if ((boxes.value[2].symbolValue === "x") && (boxes.value[4].symbolValue === "x") && (boxes.value[6].symbolValue === "x")) { winner(); return; }

    if ((boxes.value[0].value === true) && (boxes.value[1].value === true) && (boxes.value[2].value === true) && (boxes.value[3].value === true) && (boxes.value[4].value === true) && (boxes.value[5].value === true) && (boxes.value[6].value === true) && (boxes.value[7].value === true) && (boxes.value[8].value === true)) { noWinner(); return; }
}

const props = defineProps<ITicTacProps>();

onMounted(() => {
    startGame()
});


let showWinner = ref(false);
let tie = ref(false);

function noWinner() {
    tie.value = true;
    playable = false;
}

function winner() {
    showWinner.value = true;
    playable = false;
}

const onBoxClick = (i: number, personWhoCLicked: Player) => {
    if (playable) {
        if (boxes.value[i].symbolValue !== "") return

        if (props.player1.active && !props.player2.active) {
            boxes.value[i].value = true;
            boxes.value[i].symbolValue = "x";
            calculateWinner();
            props.player1.active = false;
            props.player2.active = true;
            console.log(props.player1.active)
            console.log(props.player2.active)
            return
        } else {
            boxes.value[i].value = true;
            boxes.value[i].symbolValue = "o";
            calculateWinner();
            props.player2.active = false;
            props.player1.active = true;
            console.log(props.player1.active)
            console.log(props.player2.active)
            return
        }
    }
}

const resetGame = () => {
    boxes.value = [new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, "")];
    showWinner.value = false;
    tie.value = false;
    props.player1.active = false;
    props.player2.active = false;
    startGame();
}


function startGame() {
    playable = true;
    let startNumber = Math.floor(Math.random() * (3 - 1) + 1);
    console.log(startNumber)
    if (startNumber == 1) {
        props.player1.active = true
    }
    if (startNumber == 2) {
        props.player2.active = true
    }
}


</script>


<template>
    <div class="gameboard">
    <h1>TicTacToe</h1>
    <div class="players-turn" v-if="playable && !showWinner && !tie">
        <p v-if="player1.active">It's {{ player1.username }}'s turn!</p>
        <p v-if="player2.active">It's {{ player2.username }}'s turn!</p>
    </div>
    <div class="players-turn" v-if="showWinner">
        <p v-if="player1.active">{{ player2.username }} won!</p>
        <p v-if="player2.active">{{ player1.username }} won!</p>
    </div>
    <div class="players-turn" v-if="tie">
        <p>It's a Tie!</p>
    </div>
    <div class="mainDiv">
        <div v-for="(box, index) in boxes" class="box" @click="() => {
            if (props.player1.active) {
                onBoxClick(index, props.player1)
            }
            if (props.player2.active) {
                onBoxClick(index, props.player2)
            }
        }">
            <h1>{{ box.symbolValue }}</h1>
        </div>
    </div>
   
    <div class="button-container">
       <button class="button" @click="resetGame">Restart game</button>
    </div>
   </div>
</template>

<style >

.gameboard {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.players-turn {
    margin: -40px 0 0px;
}

.mainDiv {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    width: 330px;
    height: 320px;
}

.box {
    border: black solid 5px;
    width: 100px;
    height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.button-container {
    margin: 20px;
}

p {
    font-size: 20px;
}
</style>
