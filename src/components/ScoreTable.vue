<template>
    <table class="table2">
        <tr>
            <td>Deel 1</td>
            <td></td>
            <td>Punten telling</td>
            <td>1e spel</td>
        </tr>

        <tr v-for="(counter, index) in countDices" :key="index">
            <td>{{ index }}</td>
            <td></td>
            <td>Totaal van alle {{ index }}</td>
            <td>{{ counter }}</td>
        </tr>

        <tr>
            <td>Totaal</td>
            <td>van de bovenste helft</td>
            <td>-></td>
            <td id="total-part-1">0</td>
        </tr>
    </table>

    <table class="table3">
        <tr>
            <td>Deel 2</td>
        </tr>
        <tr>
            <td>Three of a kind</td>
            <td>3 dezelfde</td>
            <td>Totaal v.d. 5 stenen</td>
            <td id="three-oak" class="part-2">0</td>
        </tr>
        <tr>
            <td>Carré</td>
            <td>4 dezelfde</td>
            <td>Totaal v.d. 5 stenen</td>
            <td id="four-oak" class="part-2">0</td>
        </tr>
        <tr>
            <td>Full House</td>
            <td>2 + 3 dezelfde</td>
            <td>25 punten</td>
            <td id="full-house" class="part-2">0</td>
        </tr>
        <tr>
            <td>Kleine straat</td>
            <td></td>
            <td>4 opeenvolgende nummers</td>
            <td id="small-straight" class="part-2">0</td>
        </tr>
        <tr>
            <td>Grote straat</td>
            <td></td>
            <td>5 opeenvolgende nummers</td>
            <td id="big-straight" class="part-2">0</td>
        </tr>
        <tr>
            <td>Topscore</td>
            <td>5 dezelfde</td>
            <td>50 punten</td>
            <td id="yahtzee" class="part-2">0</td>
        </tr>
        <tr>
            <td>Change</td>
            <td>vrije keus</td>
            <td>Totaal v.d. 5 stenen</td>
            <td id="chance" class="part-2">0</td>
        </tr>
        <tr>
            <td>Totaal</td>
            <td>van de onderste helft</td>
            <td>-></td>
            <td id="total-down">0</td>
        </tr>
        <tr>
            <td>Totaal</td>
            <td>van de bovenste helft</td>
            <td>-></td>
            <td id="total-up">0</td>
        </tr>
        <tr>
            <td>Generiek Totaal</td>
            <td></td>
            <td>-></td>
            <td id="total-score">0</td>
        </tr>
    </table>
</template>
<script setup>
import {ref, computed} from 'vue';

const dice = defineModel();
// Does not need to have computed tag because its not displayed?
const countDices = computed(() => {
    let count = {1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0};
    Object.keys(dice.value).forEach(element => {
        count[dice.value[element]]++;
    });

    return count;
});
</script>
