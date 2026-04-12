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
    

        <tr v-for="(counter, index) in countDices" :key="index">
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
            <td class="part-2">{{ determine3OAK }}</td>
        </tr>
        <tr>
            <td>Carré</td>
            <td>4 dezelfde</td>
            <td>Totaal v.d. 5 stenen</td>
            <td class="part-2">{{ determine4OAK }}</td>
        </tr>
        <tr>
            <td>Full House</td>
            <td>2 + 3 dezelfde</td>
            <td>25 punten</td>
            <td class="part-2">{{ handleFullHouse }}</td>
        </tr>
        <tr>
            <td>Kleine straat</td>
            <td></td>
            <td>4 opeenvolgende nummers</td>
            <td class="part-2">{{ handleStraight(4) }}</td>
        </tr>
        <tr>
            <td>Grote straat</td>
            <td></td>
            <td>5 opeenvolgende nummers</td>
            <td class="part-2">{{ handleStraight(5) }}</td>
        </tr>
        <tr>
            <td>Topscore</td>
            <td>5 dezelfde</td>
            <td>50 punten</td>
            <td class="part-2">{{ handleYahtZee }}</td>
        </tr>
        <tr>
            <td>Change</td>
            <td>vrije keus</td>
            <td>Totaal v.d. 5 stenen</td>
            <td class="part-2">{{ handleChance }}</td>
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
import {ref, computed} from 'vue';

const dice = defineModel();
const countDices = computed(() => {
    let count = {1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0};
    dice.value.forEach(die => {
        count[die]++
    })
    return count;
});

const calculateEyes = (index) => {
    return countDices.value[index] * index;
};

const calculateAllEyes = computed(() => {
    let sum = Object.entries(countDices.value).reduce((acc, [key, value]) => {
        return acc + (key * value);
    }, 0);
    return sum;
});

// Nog niet blij met de duplicatecode, maar probleem voor later
const determine3OAK = computed(() => {
    let found = false
    Object.values(countDices.value).forEach((value) => {
        if(value === 3){
            found = true;
        }
    })

    return found ? calculateAllEyes.value : 0;
})

const determine4OAK = computed(() => {
    let found = false
    Object.values(countDices.value).forEach((value) => {
        if(value === 4){
            found = true;
        }
    })

    return found ? calculateAllEyes.value : 0;
})

const handleFullHouse = computed(() => {
    // Check if Yahtzee reeds ingevuld is, dan telt 5 gelijke als full house?
    let threePair = false;
    let twoPair = false;

    Object.values(countDices.value).forEach((value) => {
        if (value === 3) {
            threePair = true
        }
        if (value === 2) {
            twoPair = true
        }
    })

    if (twoPair && threePair) {
        return 25
    }

    return 0;
})

const handleStraight = (requiredSize) => {
    let uniqueDices = new Set();
    Object.entries(countDices.value).forEach(([key, value]) => {
        if(value > 0){
            uniqueDices.add(key)
        }
    })

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

    switch (straightCount) {
        case 4:
            if(requiredSize === 4){
                return 30
            }
            return 0;
        case 5:
            if(requiredSize === 5){
                console.log(straightCount)
                return 40
            }
            return 0;
        default:
            return 0;
    }

}

const handleYahtZee = computed(() => {
    let foundYahtzee = false
    Object.values(countDices.value).forEach((value) => {
        if (value === 5) {
            foundYahtzee = true;
            return 50;
        }
    })
    return foundYahtzee ? 50 : 0;
});

const handleChance = computed(() => {
    let totalChance = 0
    Object.entries(countDices.value).forEach(([key, value]) => {
        totalChance = totalChance + (key * value);
    });
    return totalChance

});

const getTotalCountDown = computed(() => {
    return determine3OAK.value + determine4OAK.value +  handleFullHouse.value + handleStraight(4) + handleStraight(5) + handleYahtZee.value + handleChance.value
});

const getFullScore = computed(() => {
    return getTotalCountDown.value + calculateAllEyes.value
})

</script>
