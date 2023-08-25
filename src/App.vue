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
    startedAt: 0,
});
const timer = ref(0);
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
    settings.value.startedAt = new Date().getTime();

    statusMatch.value = "inGame";
};

// send total playtime to result scene
const onGetResult = () => {
    timer.value = new Date().getTime() - settings.value.startedAt;
    statusMatch.value = "result";
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
        @onFinish="onGetResult"
        @returnBack="statusMatch = 'menu'"
    />
    <!--Result component | statusMatch = 'result'   -->
    <ResultScene
        v-if="statusMatch === 'result'"
        :timer="timer"
        @playAgain="statusMatch = 'menu'"
    />
</template>
