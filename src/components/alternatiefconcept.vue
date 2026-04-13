<script setup>
const dice = defineModel();

const diceCount = computed(() => {
    const count = {1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0};

    dice.value.forEach(die => {
        count[die]++;
    });

    return count;
});

const isAStraight = requiredSize => {
    const currentStraight = new Set();
    let longestStraight = new Set();

    for (const key in diceCount.value) {
        if (diceCount.value[key] > 0) {
            currentStraight.add(key);
        } else {
            if (currentStraight.size > longestStraight.size) {
                longestStraight = currentStraight;
            }
            currentStraight.clear();
        }
    }
    if (currentStraight.size > longestStraight.size) {
        longestStraight = currentStraight;
    }

    return longestStraight.size === requiredSize;
};
</script>
