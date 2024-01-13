<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <play-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetReult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <p class="copyright">
    This game owned by NXT -
    <a href="https://www.facebook.com/xuantruong1510">view here</a>
  </p>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import PlayScreen from "./components/PlayScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

import { shuffled } from "./utils/array.js";

export default {
  name: "App",
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
    };
  },
  components: {
    MainScreen,
    PlayScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running " + config);
      this.settings.totalBlocks = config.totalBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalBlocks / 2 },
        (_, i) => i + 1
      );
      const cards = [...firstCards, ...firstCards];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );

      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "match";
    },
    onGetReult() {
      this.timer = new Date().getTime() - this.settings.startedAt;
      this.statusMatch = "result";
    },
  },
};
</script>

<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
