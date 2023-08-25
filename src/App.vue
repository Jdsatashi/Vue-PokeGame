<script setup>
import MainScene from "@/components/MainScene.vue";
import InteractScene from "@/components/InteractScene.vue";
import ResultScene from "@/components/ResultScene.vue";
import { ref } from "vue";
import { shuffled } from "@/utils/array";

// specific status match for specific scene
const statusMatch = ref("menu");
// settings elements in game
const settings = ref({
    totalBlock: 0,
    cardContext: [],
});
const timer = ref(0);
const points = ref(0);
const handleStart = (config) => {
    settings.value.totalBlock = config.totalBlocks;
    // get half of sum of all block by difficulty [1,2,3...7,8]
    const firstCard = Array.from(
        { length: settings.value.totalBlock / 2 },
        (_, i) => i + 1
    );
    const secondCard = [...firstCard];
    const cards = [...firstCard, ...secondCard];
    console.log(cards);
    // mixing location of cards
    settings.value.cardContext = shuffled(shuffled(shuffled(shuffled(shuffled(cards)))));

    statusMatch.value = "inGame";
};

// send total playtime to result scene
const onGetResult = (data) => {
    timer.value = data.time;
    points.value = data.points;
    statusMatch.value = 'result';
};
</script>

<template>
    <!--Menu select difficulty component | statusMatch = 'menu'   -->
    <MainScene
        v-if="statusMatch === 'menu'" @onStart="handleStart($event)"
    />
    <!--Interact component | statusMatch = 'inGame'   -->
    <InteractScene
        v-if="statusMatch === 'inGame'" :cardContext="settings.cardContext"
        @gameFinished="onGetResult($event)"
        @returnBack="statusMatch = 'menu'"
    />
    <!--Result component | statusMatch = 'result'   -->
    <ResultScene
        v-if="statusMatch === 'result'"
        :timer="timer"
        :points="points"
        @playAgain="statusMatch = 'menu'"
    />
</template>
