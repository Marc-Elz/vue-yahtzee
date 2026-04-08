<template>
    <li v-for="(item, index) in dice" :key="index">
        <!-- TODO display the dicevalues nicer than this -->
        Dice
        {{ item }}
    </li>
    <button @click="throwDice()">Gooi!</button>
</template>
<script setup>
const dice = defineModel();

const throwDice = () => {
    dice.value = Object.fromEntries(Object.keys(dice.value).map(key => [key, Math.floor(Math.random() * 6) + 1]));
};
</script>
