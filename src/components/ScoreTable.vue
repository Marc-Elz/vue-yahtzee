<template>
    <table class="table2">
   
        <!--TODO need to fix the heading of Deel 1-->
        <tr>
            <th colspan = 4>
            <td>Deel 1</td>
            <td></td>
            <td>Punten telling</td>
            <td>1e spel</td>
            </th>
        </tr>
    

        <tr v-for="(counter, index) in diceCount" :key="index">
            <td>{{ index }}</td>
            <td></td>
            <td>Totaal van alle {{ index }}</td>
            <td>{{ calculateEyes(index) }}</td>
        </tr>

        <tr>
            <td>Totaal</td>
            <td>van de bovenste helft</td>
            <td>-></td>
            <td>{{ calculateAllEyes }}</td>
        </tr>
    </table>

    <table class="table3">
        <tr>
            <th colspan="4">Deel 2</th>
        </tr>
        <tr>
            <td>Three of a kind</td>
            <td>3 dezelfde</td>
            <td>Totaal v.d. 5 stenen</td>
            <td>{{ determine3OAK }}</td>
        </tr>
        <tr>
            <td>Carré</td>
            <td>4 dezelfde</td>
            <td>Totaal v.d. 5 stenen</td>
            <td>{{ determine4OAK }}</td>
        </tr>
        <tr>
            <td>Full House</td>
            <td>2 + 3 dezelfde</td>
            <td>25 punten</td>
            <td>{{ handleFullHouse }}</td>
        </tr>
        <tr>
            <td>Kleine straat</td>
            <td></td>
            <td>4 opeenvolgende nummers</td>
            <td>{{ handleSmallStraight }}</td>
        </tr>
        <tr>
            <td>Grote straat</td>
            <td></td>
            <td>5 opeenvolgende nummers</td>
            <td>{{ handleBigStraight }}</td>
        </tr>
        <tr>
            <td>Topscore</td>
            <td>5 dezelfde</td>
            <td>50 punten</td>
            <td>{{ handleYahtZee }}</td>
        </tr>
        <tr>
            <td>Change</td>
            <td>vrije keus</td>
            <td>Totaal v.d. 5 stenen</td>
            <td>{{ handleChance }}</td>
        </tr>
        <tr>
            <td>Totaal</td>
            <td>van de onderste helft</td>
            <td>-></td>
            <td>{{getTotalCountDown }}</td>
        </tr>
        <tr>
            <td>Totaal</td>
            <td>van de bovenste helft</td>
            <td>-></td>
            <td>{{calculateAllEyes }}</td>
        </tr>
        <tr>
            <td>Generiek Totaal</td>
            <td></td>
            <td>-></td>
            <td>{{getFullScore }}</td>
        </tr>
    </table>
</template>
<script setup>
import {computed} from 'vue';

const dice = defineModel();

const diceCount = computed(() => {
    const count = {1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0};

    dice.value.forEach(die => {
        count[die]++
    })

    return count;
});

const calculateEyes = (index) => {
    return diceCount.value[index] * index;
};

const calculateAllEyes = computed(() => dice.value.reduce((acc, item) => acc + item, 0));

const isOAK = (requiredSize) => {
    for (const key in diceCount.value) {
        if(diceCount.value[key] === requiredSize) return true
    }
    return false
}

const determine3OAK = computed(() => {
    return isOAK(3) ? calculateAllEyes.value : 0;
})

const determine4OAK = computed(() => {
    return isOAK(4) ? calculateAllEyes.value : 0;
})

const handleFullHouse = computed(() => {
    // Check if Yahtzee reeds ingevuld is, dan telt 5 gelijke als full house?
    let threePair = false;
    let twoPair = false;

    for(const key in diceCount.value) {
        if (diceCount.value[key] === 3) {
            threePair = true
        }
        if (diceCount.value[key]  === 2) {
            twoPair = true
        }
    }

    if (twoPair && threePair) {
        return 25
    }

    return 0;
})

const isAStraight = (requiredSize) => {
    let uniqueDices = new Set();
    for(const key in diceCount.value){
        if(diceCount.value[key] > 0){
            uniqueDices.add(key)
        }
    }

    let orderedDices = Array.from(uniqueDices).sort();

    if (orderedDices.size < 4){
        return 0;
    }

    let straightCount = 1;
    for (let i = 0; i < orderedDices.length; i++) {
        // Check if next index is a neighbouring number
        if (orderedDices[i + 1] - orderedDices[i] === 1) {
            straightCount++;
        } else {
            if (straightCount <= 3) {
                // Reset straightcount because there might still be a straight in the set.
                straightCount = 1
            }
        }
    }

    return (straightCount === requiredSize)
}

const handleSmallStraight = computed(() => {
    return isAStraight(4) ? 30: 0;
})

const handleBigStraight = computed(() => {
    return isAStraight(5) ? 40: 0;
})

const handleYahtZee = computed(() => {
    for (const key in diceCount.value) {
        if (diceCount.value[key] === 5) {
            return 50;
        }
    }
    return 0;
});

const handleChance = computed(() => {
    return calculateAllEyes.value

});

const getTotalCountDown = computed(() => {
    return determine3OAK.value + determine4OAK.value +  handleFullHouse.value + handleSmallStraight.value + handleBigStraight.value + handleYahtZee.value + handleChance.value
});

const getFullScore = computed(() => {
    return getTotalCountDown.value + calculateAllEyes.value
})

</script>
