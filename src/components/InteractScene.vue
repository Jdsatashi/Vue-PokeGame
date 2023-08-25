<script>
import CardGame from "@/components/CardGame.vue";

export default {
    props: {
        // card context props from App.vue
        cardContext: {
            type: Array,
            default: Function
        }
    },
    components: {
        CardGame
    },
    data() {
        return {
            // rules array to save current 2 selected cards for conditions
            rules: [],
            time: 0,
            timeCounting: null,
            isProcessing: false,
            points: 0,
        };
    },
    methods: {
        // Check the rule of the game
        checkRule(card) {
            // Check to disable other cards when some cards were selected
            if (this.isProcessing) return false;

            this.rules.push(card);
            // condition if 2 cards were selected is right
            if(this.rules.length === 2){
                if(this.rules[0].index === this.rules[1].index) return this.rules = []
                if (this.rules[0].value === this.rules[1].value) {
                    console.log("true...");
                    // disable 2 right cards
                    this.$refs[`card-${this.rules[0].index}`][0].onDisable();
                    this.$refs[`card-${this.rules[1].index}`][0].onDisable();
                    // reset rules array
                    this.rules = [];

                    /* disableElements selects all elements with the class "disable" that
                     are inside elements with the classes "scene" and "card" */
                    const disableElements = document.querySelectorAll(
                        ".scene .card.disable"
                    );
                    console.log(disableElements)
                    // check disableElements exist and has length + 2 equal to cardContext length
                    // notes: disableElements.length need + 2 because first and second right cards were not added to disableElements
                    if (disableElements && disableElements.length + 2 >= this.cardContext.length) {
                        console.log(disableElements);
                        setTimeout(() => {
                            this.$emit('gameFinished', { time: this.time, points: this.points });
                        }, 1000);
                    }
                    this.updatePoints("right")
                }
                // condition if 2 cards were selected is wrong
                else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                    console.log("Wrong...");
                    // set isProcessing to prevent player spamming flip cards
                    this.isProcessing = true;
                    setTimeout(() => {
                        // flip back card selected first is wrong
                        this.$refs[`card-${this.rules[0].index}`][0].flipBackCard();
                        // flip back card selected second is wrong
                        this.$refs[`card-${this.rules[1].index}`][0].flipBackCard();
                        // reset rules array
                        this.rules = [];
                        // set back to default value allow player selecting cards

                    }, 900 /* time to acting flip back card (millisecond) */);
                    setTimeout(() => {
                        this.isProcessing = false;
                    }, 1000);
                    this.updatePoints("wrong")
                } else return false;
            }
        },
        // function send event returnBack to App.vue
        returnMenu() {
            this.$emit("returnBack");
        },
        // format time to "01:25" | 01 minute and 25 seconds
        formatTime(time) {
            const minutes = Math.floor(time / 60);
            const seconds = time % 60;
            return `${String(minutes).padStart(2, "0")}:${String(seconds).padStart(2, "0")}`;
        },
        isTimeLessThanOrEqualTo(timeToCompare, referenceTime) {
            const [compareMinutes, compareSeconds] = timeToCompare.split(":").map(Number);
            const compareTotalSeconds = compareMinutes * 60 + compareSeconds;

            const [referenceMinutes, referenceSeconds] = referenceTime.split(":").map(Number);
            const referenceTotalSeconds = referenceMinutes * 60 + referenceSeconds;

            return compareTotalSeconds <= referenceTotalSeconds;
        },
        // start counting time
        startTimer() {
            this.timeCounting = setInterval(() => {
                this.time++;
            }, 1000);
        },
        // clear timer when unmounted
        stopTimer() {
            clearInterval(this.timeCounting);
        },
        // update point depend on time flip right cards
        updatePoints(status) {
            if(status === "right"){
                const currentTime = this.formatTime(this.time);
                let fnc = this.isTimeLessThanOrEqualTo
                switch (true) {
                    case fnc(currentTime, "00:30"):
                        this.points += 50;
                        break;
                    case fnc(currentTime, "01:00"):
                        this.points += 40;
                        break;
                    case fnc(currentTime, "02:00"):
                        this.points += 30;
                        break;
                    case fnc(currentTime, "03:00"):
                        this.points += 25;
                        break;
                    case fnc(currentTime, "04:00"):
                        this.points += 20;
                        break;
                    case fnc(currentTime, "05:00"):
                        this.points += 15;
                        break;
                    case fnc(currentTime, "08:00"):
                        this.points += 10;
                        break;
                    default:
                        this.points += 5;
                        break;
                }
            } else if ( status === "wrong" && this.points > 0) return this.points -=2
        },
    },
    mounted() {
        this.startTimer();
    },
    beforeUnmount() {
        this.stopTimer();
    }
};
</script>

<template>
    <div class="scene">
        <div class="flex">
            <div>
                <h1>Flip 2 same card to play. (test 2)</h1>
            </div>
            <div>
                <div class="flex">
                    <p style="font-size: 1.5em">Time: {{ formatTime(time) }}</p>
                    <p style="font-size: 1.5em; margin-left: 1.25em">Points: {{ points }}</p>
                    <button class="btn" style="margin-left: 1.25em" @click="returnMenu">Back</button>
                </div>
            </div>
        </div>
        <div class="scene-inner" :style="{
            width: `${(((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4) + 16) * Math.sqrt(cardContext.length)}px`
        }">
            <CardGame v-for="(card, index) in cardContext"
                      :key="index"
                      :ref="`card-${index}`"
                      :imgBackFaceUrl="`${card}`"
                      v-bind:card="{ index, value: card }"
                      :rules="rules"
                      :card-context="cardContext"
                      @flip="checkRule($event)"
                      :class="{ 'disabled-card': isProcessing }"
            />
        </div>
    </div>
</template>

<style scoped>
.disabled-card {
    cursor: not-allowed;
    pointer-events: none;
}

.scene {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light)
}

.scene-inner {
    display: flex;
    flex-wrap: wrap;
    margin: 0.5em auto;
}

.flex {
    display: flex;
    justify-content: space-around;
}

.btn {
    padding: 8px 20px;
    background-color: #ffffff;
    border: 1px solid rgba(155, 153, 153, 0.9);
    color: #151515;
    cursor: pointer;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 600;
}

.btn:hover {
    background-color: #ebf7ff;
    color: #151515;
    border: 1px solid rgba(155, 153, 153, 0.9);
}
</style>
