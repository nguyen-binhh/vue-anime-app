<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array.js";
export default {
  emits: ["onStart"],
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  data() {
    return {
      settings: {
        totalOfCards: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
      statusMatch: "default",
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("Running handle before start", config);
      this.settings.totalOfCards = config.totalOfCards;

      const firstCards = Array.from(
        // 2 mảng từ 1 -> 8
        {
          length: this.settings.totalOfCards / 2,
        },
        // cách nhau 1 đơn vị
        (_, i) => i + 1
      );
      // console.log(firstCards);

      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      // console.log(cards);

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      // console.log(this.settings.cardsContext);

      this.settings.startedAt = new Date().getTime();

      // data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      this.statusMatch = "result";
    },
  },
};
</script>
