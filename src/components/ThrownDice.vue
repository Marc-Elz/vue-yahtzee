<template>
    <table class="table1">
        <th colspan="5">Dice values</th>
        <tr>
            <td v-for="(item, index) in dice" :key="index">
                <!-- TODO display the dicevalues nicer than this -->
                {{ item }}
            </td>
        </tr>
    </table>
    <button @click="throwDice()">Gooi!</button>
</template>
<script setup>
const dice = defineModel();

const throwDice = () => {
    dice.value = dice.value.map(() => Math.floor(Math.random() * 6) + 1);
};
</script>
