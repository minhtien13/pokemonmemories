<template>
  <div
    class="cart"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cartContexts.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cartContexts.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((920 - 16 * 4) / Math.sqrt(cartContexts.length) - 16) * 3 * 2
      }px`,
    }"
  >
    <div
      class="cart__inner"
      :class="{ 'is-flipped': isToggleFlipCart }"
      @click="onToggleFlippedCart"
    >
      <div
        class="cart__face cart__face--front"
        :style="{
          backgroundSize: `${
            (((920 - 16 * 4) / Math.sqrt(cartContexts.length) - 16) * 3) / 4 / 3
          }px`,
        }"
      >
        <div class="cart_content"></div>
      </div>
      <div class="cart__face cart__face--back">
        <div
          class="cart_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imaBackUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imaBackUrl: {
      type: String,
      require: true,
    },
    card: {
      type: [String, Number, Object, Array],
    },
    cartContexts: {
      type: [Object, Array],
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isToggleFlipCart: false,
    };
  },
  methods: {
    onToggleFlippedCart() {
      if (this.isDisabled) return false;
      this.isToggleFlipCart = !this.isToggleFlipCart;
      if (this.isToggleFlipCart) this.$emit("onFlip", this.card);
    },
    onBackFlip() {
      this.isToggleFlipCart = false;
    },
    onEnaBledMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.cart {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.cart__inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
}
.cart.disabled .cart__inner {
  cursor: default;
}
.cart__inner.is-flipped {
  transform: rotateY(-180deg);
}
.cart__face--back {
  transform: rotateY(-180deg);
  background-color: var(--light);
}
.cart__face--back .cart_content {
  width: 100%;
  height: 100%;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center center;
}
.cart__face--front {
  background: url("@/assets/images/icon_back.png") no-repeat;
  background-position: center center;
}
.cart__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
</style>
