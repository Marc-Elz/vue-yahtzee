<script setup>
import ScoreTable from './components/ScoreTable.vue';
import ThrownDice from './components/ThrownDice.vue';
import {ref} from 'vue';

const dice = ref({
    1: 1,
    2: 2,
    3: 5,
    4: 1,
    5: 6,
});
</script>

<template>
    <ThrownDice v-model="dice" />
    <ScoreTable v-model="dice" />
</template>
