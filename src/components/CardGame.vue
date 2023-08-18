/*
import { ref } from "vue";

const emits = defineEmits(["flip"]);

const isFlipped = ref(false);

// eslint-disable-next-line vue/valid-define-props
const { card, imgBackFaceUrl } = defineProps({
card: [String, Number, Array, Object],
imgBackFaceUrl: String
});

const onToggleFlipCard = () => {
isFlipped.value = !isFlipped.value;
if (isFlipped.value) {
emits("flip", card);
}
};
const onFlipBackCard = () => {
isFlipped.value = false;
}
*/
<script>
    export default {
        props: {
            card: {
                type: [String, Number, Array, Object]
            },
            imgBackFaceUrl: {
                type: String,
                required: true
            }
        },
        data() {
            return {
                isFlipped: false
            };
        },

        methods: {
            onToggleFlipCard() {
                this.isFlipped = !this.isFlipped;
                if(this.isFlipped) this.$emit("flip", this.card)
            },
            onFlipBackCard(){
                this.isFlipped = false
            }
        }
    }
</script>

<template>
    <div class="card">
        <div class="card-inner"
             :class="{'is-flipped': isFlipped}"
             v-on:click="onToggleFlipCard"
        >
            <div class="card-face card_face-font">
                <div class="card-content"></div>
            </div>
            <div class="card-face card_face-back">
                <div class="card-content"
                     :style="{ backgroundImage: `url('/src/assets/${imgBackFaceUrl}')`}">
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="css" scoped>
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    width: 90px;
    height: 120px;
}

.card-inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}

.card-inner.is-flipped {
    transform: rotateY(-180deg);
}

.card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.3);
}

.card_face-font .card-content {
    background: url("../assets/images/icon_back.png") no-repeat center center;
    background-size: 40px 40px;
    width: 100%;
    height: 100%;
}

.card_face-back {
    background-color: var(--light);
    transform: rotateY(-180deg);
}

.card_face-back .card-content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}
</style>
