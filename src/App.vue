<script setup>
import ScoreTable from './components/ScoreTable.vue';
import ThrownDice from './components/ThrownDice.vue';
import {ref} from 'vue';

const dice = ref({
    1: 1,
    2: 1,
    3: 1,
    4: 2,
    5: 2,
});
</script>

<template>
    <ThrownDice v-model="dice" />
    <ScoreTable v-model="dice" />
</template>
