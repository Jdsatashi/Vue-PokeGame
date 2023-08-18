/*
import CardGame from "@/components/CardGame.vue";
import { ref } from "vue";

const rules = ref([]);

defineProps({
cardContext: {
type: Array,
default: () => []
}
});

const checkRule = (card) => {
let rule = rules.value;
if (rule.length === 2) return false;
rule.push(card);

if (rule.length === 2 && rule[0].value === rule[1].value) {
console.log("true...");
} else if (rule.length === 2 && rule[0].value !== rule[1].value) {
console.log("Wrong...");
ref.value[`card-${rule[0].index}`].onFlipBackCard();
ref.value[`card-${rule[1].index}`].onFlipBackCard();
} else return false;
};
*/
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
            } else if(this.rules.length === 2 && this.rules[0].value !== this.rules[1].value){
                console.log("Wrong...")
                this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

                this.rules = []
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
