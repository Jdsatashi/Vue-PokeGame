<script setup>
import { defineEmits } from "vue";

const emits = defineEmits(["onStart"]);

const onClickStart = (totalBlocks) => {
    emits("onStart", { totalBlocks });
};
</script>

<template>
    <div class="screen">
        <h1>POKEMON MEMORIES</h1>
        <h3>Select mode to start game</h3>
        <div class="actions">
            <button v-on:click="onClickStart(16)">
                <span>4x4</span>
                <span>Easy</span>
            </button>
            <button v-on:click="onClickStart(36)">
                <span>6x6</span>
                <span>Normal</span>
            </button>
            <button v-on:click="onClickStart(64)">
                <span>8x8</span>
                <span>Hard</span>
            </button>
            <button v-on:click="onClickStart(100)">
                <span>10x10</span>
                <span>Super hard</span>
            </button>
        </div>
    </div>
</template>

<style scoped>

</style>
