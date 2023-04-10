<template>
  <main-screen
    v-if="matchStatus === 'default'"
    @onStart="($event) => onBeforeStart($event)"
  />
  <interact-screen
    v-if="matchStatus === 'match'"
    :cardsContext="settings.cardSContext"
    @onFinish="checkResult()"
  />
  <Result-screen
    v-if="matchStatus === 'result'"
    :timer="timer"
    @onStartAgain="matchStatus = 'default'"
  />
  <copy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { suffled } from "./utils/array";
import CopyRightScreen from "./components/CopyRightScreen.vue";

export default {
  name: "App",
  data() {
    return {
      matchStatus: "default",
      settings: {
        totalOfBlocks: 0,
        cardSContext: [],
        startAt: null,
        timer: null,
      },
    };
  },
  methods: {
    onBeforeStart(e) {
      this.settings.totalOfBlocks = e.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      this.settings.cardSContext = suffled(
        suffled(suffled([...firstCards, ...firstCards]))
      );
      this.settings.startAt = new Date().getTime();
      this.matchStatus = "match";
    },
    checkResult() {
      this.matchStatus = "result";
      this.timer = new Date().getTime() - this.settings.startAt;
    },
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
};
</script>
