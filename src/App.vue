<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onStartBefore($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cartContexts="settings.cartContext"
    @onFinish="onFinish"
  />
  <resuit-screen
    v-if="statusMatch === 'winner'"
    :timer="timer"
    @resetAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResuitScreen from "./components/ResuitScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  components: { MainScreen, InteractScreen, ResuitScreen },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlogs: 0,
        cartContext: [],
        started: null,
      },
      timer: 0,
    };
  },
  methods: {
    onStartBefore(config) {
      this.settings.totalOfBlogs = config.totalOfBlog;
      const firstCarts = Array.from(
        { length: this.settings.totalOfBlogs / 2 },
        (_, i) => i + 1
      );
      const secondCarts = [...firstCarts];
      const carts = [...firstCarts, ...secondCarts];
      this.settings.cartContext = shuffled(shuffled(shuffled(carts)));
      this.settings.started = new Date().getTime();
      this.statusMatch = "match";
    },
    onFinish() {
      this.timer = new Date().getTime() - this.settings.started;
      this.statusMatch = "winner";
    },
  },
};
</script>

<style></style>
