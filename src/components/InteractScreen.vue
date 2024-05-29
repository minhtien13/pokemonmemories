<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cartContexts.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cartContexts.length)
        }px`,
      }"
    >
      <cart-flip
        v-for="(cart, index) in cartContexts"
        :key="index"
        :ref="`card-${index}`"
        :imaBackUrl="`images/${cart}.png`"
        :cartContexts="cartContexts"
        :card="{ index, value: cart }"
        @onFlip="checkRules($event)"
      />
    </div>
  </div>
</template>

<script>
import Cart from "./Cart.vue";
export default {
  props: {
    cartContexts: {
      type: [Object, Array],
      default: function () {
        return [];
      },
    },
  },
  components: {
    CartFlip: Cart,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRules(cart) {
      const { rules } = this;

      if (rules.length === 2) return false;
      rules.push(cart);

      if (rules.length === 2 && rules[0].value === rules[1].value) {
        this.$refs[`card-${rules[0].index}`][0].onEnaBledMode();
        this.$refs[`card-${rules[1].index}`][0].onEnaBledMode();
        this.rules = [];

        const onDisabledAllElements = document.querySelectorAll(
          ".screen .cart.disabled"
        );
        if (
          onDisabledAllElements &&
          onDisabledAllElements.length === this.cartContexts.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (rules.length === 2 && rules[0].value !== rules[1].value) {
        setTimeout(() => {
          this.$refs[`card-${rules[0].index}`][0].onBackFlip();
          this.$refs[`card-${rules[1].index}`][0].onBackFlip();
          this.rules = [];
        }, 800);
        console.log("Wrong...");
      }
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
  background-color: var(--dark);
  color: var(--light);
}
.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
