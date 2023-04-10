<template lang="">
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is--flipped': isFlipped }"
      @click="onToggleCard"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            'background-size': `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    card: {
      type: [Number, Object],
    },
    rules: {
      type: [String, Object],
    },
  },
  methods: {
    onToggleCard() {
      if (this.isDisabled || this.rules.length === 2) return false;
      this.isFlipped = true;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipCard() {
      this.isFlipped = false;
    },
    onDisnabledCard() {
      this.isDisabled = true;
    },
  },
};
</script>
<style lang="css" scoped>
.card {
  display: inline-block;
  margin: 0 1rem 1rem 0;
}
.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  position: relative;
  cursor: pointer;
}
.card__inner.is--flipped {
  transform: rotateY(180deg);
}
.card.disabled .card__inner {
  cursor: default;
}
.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
.card__face--back {
  background: var(--light);
  transform: rotateY(180deg);
}
.card__face--front .card__content {
  background: url(../assets/images/icon_back.png) no-repeat center center;
  height: 100%;
  width: 100%;
}
.card__face--back .card__content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
</style>
