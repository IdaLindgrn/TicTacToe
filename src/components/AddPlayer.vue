<script setup lang="ts">
import { ref, computed } from "vue";

const x = ref("");
const o = ref("");

const isSaveDisabled = computed(() => {
  return x.value === "" || o.value === "";
});

defineEmits(["addPlayers"]);


</script>

<template>
    <form>
        <h1>TicTacToe</h1>
    <div class="input-container">
      <div class="input-field">
        <label for="x">Player X</label>
        <input class="input" v-model="x">
      </div>
      <div class="input-field">
        <label for="o">Player O</label>
        <input class="input" v-model="o">
      </div>
    </div>
        <button class="button" :disabled="isSaveDisabled" @click="$emit('addPlayers', x, o)">Start Game</button>
    </form>
</template>

<style>

.input-container {
    margin-top: -15px;
    display: flex;
    flex-direction: column;
}

.input-field {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
}

.input-field label {
  text-align: left;
  margin-bottom: 5px;
  font-size: 1.2em;
  font-weight: bold;
}

.input {
    width: 250px;
    padding: 13px 20px;
    border-radius: 6px;
    background-color: rgb(239, 242, 246);
    text-decoration: none;
    border-color: black;
    font-size: 17px;
}

.button {
    margin-top: 10px;
    width: 180px;
    padding: 13px;
    background-color: rgb(191, 202, 213);
    margin: 10px;
    font-size: 18px;
    font-weight: bold;
    cursor: default;
    border: none;
    outline: none;
}

.button:not(:disabled):hover {
  background-color: rgb(175, 186, 196);
  cursor: pointer;
}

.save-button:disabled {
  background-color: rgb(216, 218, 224); 
  cursor: not-allowed;

}

</style>