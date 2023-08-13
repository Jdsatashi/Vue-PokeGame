<script setup>
    import CardGame from "@/components/CardGame.vue";

    defineProps({
        cardContext: {
            type: Array,
            default: () => []
        }
    })
</script>

<template>
    <div class="scene">
        <h1>Interact Component here</h1>
        <CardGame v-for="(card, index) in cardContext" :key="index"  :img-back-face-url="`images/${card}.png`"/>
    </div>
</template>

<style scoped>

</style>
