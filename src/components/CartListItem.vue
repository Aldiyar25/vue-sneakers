<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const { cart, removeFromCart } = inject('cart');

const onClickRemove = () => {
  addToCart(item);
};
</script>

<template>
  <div class="flex flex-col gap-4 justify-between" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>

<style lang="scss" scoped></style>
