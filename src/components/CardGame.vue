<script>
export default {
    props: {
        card: {
            type: [String, Number, Array, Object]
        },
        imgBackFaceUrl: {
            type: String,
            required: true
        },
        cardContext: {
            type: Array,
            default: Function
        }
    },
    data() {
        return {
            isFlipped: false,
            isDisabled: false,
        };
    },
    methods: {
        // flip front to back card function
        onToggleFlipCard() {
            if (this.isDisabled) return false;
            this.isFlipped = true;
            if(this.isFlipped) {
                this.$emit("flip", this.card)
                this.onDisable()
            }
        },
        // flip back to close card function
        flipBackCard(){
            this.isFlipped = false
            this.isDisabled = false
        },
        // disable cards which were right
        onDisable(){
            this.isDisabled = true;
        },
        // get specific image url
        imgUrl() {
            // dynamic url setup > https://vitejs.dev/guide/assets.html#new-url-url-import-meta-url
            return new URL(`../assets/images/${this.imgBackFaceUrl}.png`, import.meta.url).href
        }
    }
}
</script>

<template>
    <div class="card" :class="{ 'disable': isDisabled }"
         :style="{
            height: `${(920 - 16 * 4) / Math.sqrt(cardContext.length) - 20}px`,
            width: `${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4}px`,
            perspective: `${(((920 - 16 * 4) / (cardContext.length) - 16) * 3) / (0.5 * (0.5 / cardContext.length))}px`,
            borderRadius: `${16 * (16 / cardContext.length)}px`
    }">
        <div class="card-inner"
             :class="{'is-flipped': isFlipped}"
             v-on:click="onToggleFlipCard"
        >
            <div class="card-face card_face-font">
                <div class="card-content" :style="{
                    backgroundSize: `${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 / 3}px`
                }"></div>
            </div>
            <div class="card-face card_face-back" :style="{borderRadius: `${16 * (16 / cardContext.length)}px`}">
                <div class="card-content"
                     :style="{
                        'background-image': `url(${imgUrl()})`,
                      }"
                     >
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
    border: 1px solid rgba(155, 153, 153, 0.9);
    background: #363434 linear-gradient(45deg, #1f1a1a, rgba(131, 129, 129, 0.9) 50%, #2d2623);
}

.card-inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}

.card.disabled .card-inner{
    cursor: default;
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
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.3);
}

.card_face-font .card-content {
    background: url("@/assets/images/icon_back.png") no-repeat center center;
    width: 100%;
    height: 100%;
}

.card_face-back {
    background: #f5f5f5 linear-gradient(125deg, #f8f8f8, rgba(164, 164, 164, 0.91) 50%, #fafafa);
    transform: rotateY(-180deg);
    width: 100%;
    height: 100%;
}

.card_face-back .card-content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}
</style>
