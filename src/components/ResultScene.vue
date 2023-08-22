<script>
export default {
    props: {
        timer: {
            type: Number,
            required: true
        }
    },
    data() {
        return {};
    },
    methods: {
        playAgain() {
            this.$emit("playAgain");
        }
    }
};
</script>

<template>
    <div class="screen">
        <h1>Congratulations</h1>
        <p>{{ Math.round(timer / 1000) }} seconds</p>
        <button @click="playAgain">Play again</button>
    </div>
</template>

<style scoped>

</style>
