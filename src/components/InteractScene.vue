<script>
import CardGame from "@/components/CardGame.vue";

export default {
    props: {
        cardContext: {
            type: Array,
            default: function () { return [] }
        }
    },
    components: {
        CardGame
    },
    data() {
        return {
            rules: []
        };
    },
    methods: {
        checkRule(card){
            console.log(card)
            if(this.rules.length === 2) return false
            this.rules.push(card)

            if(this.rules.length === 2 && this.rules[0].value === this.rules[1].value){
                console.log("true...")
                this.$refs[`card-${this.rules[0].index}`][0].onDisable();
                this.$refs[`card-${this.rules[1].index}`][0].onDisable();
                this.rules = []

                const disableElements = document.querySelectorAll(
                    ".scene .card.disable"
                )
                if (disableElements && disableElements.length + 2 === this.cardContext.length){
                    setTimeout(() => {
                        this.$emit("onFinish")
                    }, 1000)
                }
            } else if(this.rules.length === 2 && this.rules[0].value !== this.rules[1].value){
                console.log("Wrong...")
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].flipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].flipBackCard();

                    this.rules = []
                }, 900)

            } else return false
        }
    }
};
</script>

<template>
    <div class="scene">
        <h1>Interact Component here</h1>
        <CardGame v-for="(card, index) in cardContext"
                  :key="index"
                  :ref="`card-${index}`"
                  :img-back-face-url="`images/${card}.png`"
                  v-bind:card="{ index, value: card }"
                  :rules="rules"
                  @flip="checkRule($event)"
        />
    </div>
</template>

<style scoped>

</style>
