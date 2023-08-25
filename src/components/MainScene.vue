<script setup>
const emits = defineEmits(["onStart"]);
// send number of blocks to App.vue
const onClickStart = (totalBlocks) => {
    emits("onStart", { totalBlocks });
};
</script>

<template>
    <div class="screen">
        <div class="card">
            <h1 class="title">POKEMON MEMORIES</h1>
            <h3 class="subtitle">Select mode to start game</h3>
            <div class="actions">
                <div class="button-row">
                    <button class="btn" v-on:click="onClickStart(16)">
                        <span class="button-text">4x4</span>
                        <span class="button-description">Easy</span>
                    </button>
                    <button class="btn" v-on:click="onClickStart(36)">
                        <span class="button-text">6x6</span>
                        <span class="button-description">Normal</span>
                    </button>
                </div>
                <div class="button-row">
                    <button class="btn" v-on:click="onClickStart(64)">
                        <span class="button-text">8x8</span>
                        <span class="button-description">Hard</span>
                    </button>
                    <button class="btn" v-on:click="onClickStart(100)">
                        <span class="button-text">10x10</span>
                        <span class="button-description">Super hard</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.screen {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.card {
    background-color: white;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    text-align: center;
    width: 80%;
    max-width: 400px;
    margin-top: -5em;
}

.title {
    font-size: 2.5em;
    font-weight: 700;
    background: linear-gradient(45deg, #00A5FF, rgba(152, 223, 245, 0.98) 50%, #00A5FF);
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    text-shadow: 0.25em 0.25em 0.5em rgba(159, 157, 157, 0.9);
    transform: perspective(50px) rotateX(10deg);
}

.subtitle {
    font-size: 1.5rem;
    margin-top: 10px;
    text-align: center;
    text-shadow: 2px 2px 4px rgba(157, 155, 155, 0.9);
    border: 1px solid rgba(0, 0, 0, 0.9);
    padding: 10px;
    opacity: 0.9;
    border-radius: 0.5rem;
}

.actions {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 20px;
}

.button-row {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
    margin-top: 10px;
}

.button-text {
    font-weight: bold;
    margin-right: 5px;
}

.button-description {
    font-weight: bold;
}

.btn {
    margin: 10px;
    padding: 10px 20px;
    background: #00A5FF linear-gradient(45deg, #00A5FF, rgba(152, 223, 245, 0.98) 50%, #00A5FF);
    border: none;
    color: white;
    cursor: pointer;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.btn:hover {
    background: #0f84c0 linear-gradient(45deg, #0f84c0, rgba(149, 221, 243, 0.98) 50%, #0f84c0);
    color: white;
}
</style>

