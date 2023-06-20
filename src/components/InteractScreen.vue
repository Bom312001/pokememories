<template>
  <div class="screen">
    <!-- <h1>InteractScreen componet</h1> -->
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
            4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <CardFlip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imageUrls="`/src/assets/images/${card}.png`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRules($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";
export default {
  props: { cardsContext: Array },
  data() {
    return {
      rules: [],
      widthWd: screen.width - 120,
      heightWd: screen.height - 120,
    };
  },
  components: {
    CardFlip,
  },
  methods: {
    checkRules(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      // console.log(this.rules);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Right....");
        // add class 'disable' to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisabledMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisabledMode();

        //reset rules =[]
        this.rules = [];

        const disableElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        console.log(disableElements);
        if (
          disableElements &&
          disableElements.length === this.cardsContext.length - 2
        ) {
          console.log("endgame");
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("Woring....");
        //close two card
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          //reset rules = 0
          this.rules = [];
        }, 500);
      } else return false;
    },
  },
  mounted() {
    console.log(this.cardsContext);
    console.log(this.heightWd);
  },
  updated() {
    console.log(this.$refs[`card-${this.rules[0].index}`]);
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
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  /* width: 424px; */
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
  /* max-height: calc(100% - 4rem);
  overflow: hidden; */
}
</style>
