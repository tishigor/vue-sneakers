<script setup>
import DrawerHead from "@/components/DrawerHead.vue";
import CartItemList from "@/components/CartItemList.vue";
import InfoBlock from "@/components/InfoBlock.vue";

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean,
})
</script>

<template>
  <div class="drawer"></div>
  <div class="drawer__block">
    <DrawerHead/>

    <div v-if="!totalPrice" class="drawer__block__empty">
      <InfoBlock
          title="Корзина пустая"
          description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
          image-url="/package-icon.png"/>
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
            @click="() => emit('createOrder')"
            class="drawer__block__total__button">
          Оформить заказ
        </button>
      </div>
    </div>


  </div>
</template>

<style scoped>

</style>