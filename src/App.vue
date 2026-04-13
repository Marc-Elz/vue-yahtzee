<script setup>
import ScoreTable from './components/ScoreTable.vue';
import ThrownDice from './components/ThrownDice.vue';
import {ref} from 'vue';

// Better make the dice object an array
const dice = ref([1, 3, 4, 5, 6]);
</script>

<template>
    <ThrownDice v-model="dice" />
    <ScoreTable v-model="dice" />
</template>
