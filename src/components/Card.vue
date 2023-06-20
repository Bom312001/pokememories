<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`, // CR là 3. CD là 4
      perspective: `${
        ((((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
          4) *
        2
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) *
                3) /
              4 /
              3
            }px ${
              (((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) *
                3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card__content" :style="backgroundImageInlineStyle"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imageUrls: String,
    card: {
      type: [Number, String, Object, Array],
    },
    cardsContext: Array,
  },
  data() {
    return {
      widthWd: screen.width - 120,
      heightWd: screen.height - 120,
      isDisabled: false,
      isFlipped: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnabledDisabledMode() {
      this.isDisabled = true;
    },
  },
  computed: {
    backgroundImageInlineStyle() {
      return `background-image: url("${this.imageUrls}");`;
    },
  },
  mounted() {
    // console.log(this.imageUrls);
    // console.log(this.card);
  },
};
</script>
<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  /* width: 90px;
  height: 120px; */
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  /* css để coi mặt trước cx giống mặt sau */
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  /* background-size: 40px 40px; */
  height: 100%;
  width: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}

.card.disabled .card__inner {
  cursor: default;
}
</style>
