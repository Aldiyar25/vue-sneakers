<script setup>
import { ref, inject, computed } from "vue";
import axios from "axios";
import DrawerHead from "./DrawerHead.vue";
import CartListItem from "./CartListItem.vue";
import InfoBlock from "./InfoBlock.vue";

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
});

const { cart, closeDrawer } = inject("cart");

const isCreating = ref(false);
const orderId = ref(null);
const createOrder = async () => {
  try {
    isCreating.value = true;
    const { data } = await axios.post(
      `https://742c758d2468301e.mokky.dev/orders`,
      {
        items: cart.value,
        totalPrice: props.totalPrice.value,
      }
    );

    cart.value = [];

    orderId.value = data.id;
  } catch (err) {
    console.log(err);
  } finally {
    isCreating.value = false;
  }
};

const cartIsEmpty = computed(() => {
  cart.value.length === 0;
});
const buttonDisabled = computed(() => {
  isCreating.value || cartIsEmpty.value;
});
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ №${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartListItem />

      <div v-if="totalPrice" class="flex flex-col gap-5 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} тг</b>
        </div>
        <!-- flex gap-2 -->
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} тг</b>
        </div>
        <!-- flex gap-2 -->

        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
