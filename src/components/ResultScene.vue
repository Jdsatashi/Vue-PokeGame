<script>
export default {
    props: {
        timer: {
            type: [Number, String],
            required: true
        },
        points: {
            type: [Number, String],
            required: true
        }
    },
    data() {
        return {};
    },
    methods: {
        // function to return menu scene
        playAgain() {
            this.$emit("playAgain");
        }
    }
};
</script>

<template>
    <div class="screen">
        <div class="centered-card">
            <h1>Congratulations</h1>
            <p style="font-size: 1.5rem">Points: <b>{{ points }}</b> | Total time: <b>{{ timer }}</b>s</p>
            <button @click="playAgain">Play again</button>
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

.centered-card {
    background-color: #ffffff;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    text-align: center;
    width: 500px;
    margin-top: -5em;
}

.centered-card h1 {
    font-size: 3rem;
    color: #00A5FF;
    margin-bottom: 10px;
}

.centered-card p {
    font-size: 1.2rem;
    margin-bottom: 20px;
}

.centered-card button {
    background-color: #00A5FF;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1rem;
}
</style>

