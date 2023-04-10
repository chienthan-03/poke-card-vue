<template lang="">
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-play
        v-for="(card, index) in cardsContext"
        :key="index"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        :ref="`card-${index}`"
        :rules="rules"
        @onFlip="($event) => checkRule($event)"
      />
    </div>
  </div>
</template>
<script>
import CardPlay from "./CardPlay.vue";

export default {
  components: {
    CardPlay,
  },
  props: {
    cardsContext: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (
        this.rules.length === 1 &&
        this.rules[0] &&
        this.$refs[`card-${this.rules[0].index}`][0].card === card
      ) {
        return false;
      }

      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        //add class disabled to card
        this.$refs[`card-${this.rules[0].index}`][0].onDisnabledCard();
        this.$refs[`card-${this.rules[1].index}`][0].onDisnabledCard();
        //reset rules
        this.rules = [];
        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 800);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          //clouse card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipCard();
          //reset rules
          this.rules = [];
        }, 360);
      } else return false;
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: #212121;
  color: #f3f3f3;
}
.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
