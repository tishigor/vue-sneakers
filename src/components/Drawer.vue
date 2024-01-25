<script setup>
import DrawerHead from "@/components/DrawerHead.vue";
import CartItemList from "@/components/CartItemList.vue";
import InfoBlock from "@/components/InfoBlock.vue";
import {computed, inject, ref} from "vue";
import axios from "axios";

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean,
})

const {cart, closeDrawer} = inject('cart')

const isCreating = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isCreating.value = true;
    const {data} = await axios.post(`https://5324a7cf8080ae0b.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value,
    })
    cart.value = [];
    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false;
  }
}


const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)


</script>

<template>
  <div class="drawer"></div>
  <div class="drawer__block">
    <DrawerHead/>


    <div v-if="!totalPrice || orderId" class="drawer__block__empty">
      <InfoBlock
          v-if="!totalPrice && !orderId"
          title="Корзина пустая"
          description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
          image-url="/package-icon.png"
      />
      <InfoBlock
          v-if="orderId"
          title="Заказ оформлен!"
          :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
          image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList/>

      <div class="drawer__block__total">

        <div class="drawer__block__total__line">
          <span>Итого:</span>
          <div class="drawer__block__total__line__space-between"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>

        <div class="drawer__block__total__line">
          <span> Налог 5%:</span>
          <div class="drawer__block__total__line__space-between"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>

        <button
            :disabled="buttonDisabled"
            @click="createOrder"
            class="drawer__block__total__button">
          Оформить заказ
        </button>
      </div>
    </div>


  </div>
</template>

<style scoped>

</style>