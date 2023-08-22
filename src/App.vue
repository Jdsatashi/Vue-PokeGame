<script setup>
    import MainScene from '@/components/MainScene.vue';
    import InteractScene from '@/components/InteractScene.vue';
    import ResultScene from '@/components/ResultScene.vue';
    import { ref } from "vue";
    import { shuffled } from "@/utils/array";

    const statusMatch = ref("default");
    const settings = ref({
        totalBlock: 0,
        cardContext: [],
        startedAt: 0
    });
    const timer = ref(0)
    const handleStart = (config) => {
        let totalBlock =  settings.value.totalBlock
        totalBlock = config.totalBlocks
        const firstCard = Array.from(
            {length: totalBlock / 2},
            (_, i) => i + 1,
        )
        const secondCard = [...firstCard]
        const cards = [...firstCard, ...secondCard]
        console.log(cards)
        settings.value.cardContext = shuffled(shuffled(shuffled(shuffled(shuffled(cards)))))
        settings.value.startedAt = new Date().getTime()

        statusMatch.value = "inGame";
    }

    const onGetResult = () => {
        timer.value = new Date().getTime() - settings.value.startedAt

        statusMatch.value = "result"
    }
</script>

<template>
    <MainScene
        v-if="statusMatch === 'default'" @onStart="handleStart($event)"
    />
    <InteractScene
        v-if="statusMatch === 'inGame'" :cardContext="settings.cardContext"
        @onFinish="onGetResult"
    />
    <ResultScene
        v-if="statusMatch === 'result'"
        :timer="timer"
        @playAgain="statusMatch = 'default'"
    />
</template>

<style scoped>

</style>
