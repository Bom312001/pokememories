<template>
  <MainScreen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <InteractScreen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <ResultScreen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <CopyRightScreenVue />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreenVue from "./components/CopyRightScreen.vue";
import { shuflled } from "./utils/array";

export default {
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreenVue,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },

  methods: {
    onHandleBeforeStart(config) {
      console.log("running handle before start, ", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      console.log(firstCards);

      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      console.log(cards);

      this.settings.cardsContext = shuflled(shuflled(cards));
      console.log(this.settings.cardsContext);

      this.settings.startedAt = new Date().getTime();

      //data ready
      this.statusMatch = "match";
    },

    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;

      //switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>

<style scoped></style>
