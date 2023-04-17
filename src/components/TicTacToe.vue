<script setup lang="ts">
import { Box } from "../models/Box";
import { StyleValue, ref } from "vue";
import { Player } from "../models/Player";
import { computed } from "@vue/reactivity";

let boxes = ref([new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, "")]);

interface ITicTacProps {
    player1: Player;
    player2: Player;
}

const calculateWinner = (squares: Box[]) => {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ];
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
        }
        return null;
    }
}

const winner = computed(() => calculateWinner(boxes.value.flat()))



const props = defineProps<ITicTacProps>();

const onBoxClick = (i: number, personWhoCLicked: Player) => {
    if (winner.value) return { winner }
    console.log(winner.value)
    if (boxes.value[i].symbolValue !== "") return

    boxes.value[i].value = true;
    boxes.value[i].symbolValue = personWhoCLicked.role;

    personWhoCLicked.role = personWhoCLicked.role === 'x' ? 'o' : 'x';
    personWhoCLicked.active = personWhoCLicked.active === false;
    !personWhoCLicked.active != personWhoCLicked.active === true;
    console.log(personWhoCLicked.active)
    console.log(personWhoCLicked.username)

    console.log(!personWhoCLicked.active)
    console.log(!personWhoCLicked.username)


}

const resetGame = () => {
    boxes.value = [new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, ""), new Box(false, "")];
}


function startGame() {
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
    <h1>TicTacToe</h1>
    <p v-if="player1.active">Det är {{ player1.username }}s tur!</p>
    <p v-if="player2.active">Det är {{ player2.username }}s tur!</p>
    <button @click.once="(startGame())">Start Game</button>
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
    <h2 v-if="winner">Spelare '{{ winner }} vinner!'</h2>
    <button @click="resetGame">STARTA OM</button>
</template>

<style scoped >
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
}
</style>
