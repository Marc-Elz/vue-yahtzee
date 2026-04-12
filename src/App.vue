<script setup>
import ScoreTable from './components/ScoreTable.vue';
import ThrownDice from './components/ThrownDice.vue';
import {ref} from 'vue';

// Better make the dice object an array
const dice = ref([1, 1, 1, 2, 2]);
</script>

<template>
    <ThrownDice v-model="dice" />
    <ScoreTable v-model="dice" />
</template>
