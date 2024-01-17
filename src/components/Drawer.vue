<script setup>

import DrawerHead from "@/components/DrawerHead.vue";
import CartItemList from "@/components/CartItemList.vue";
</script>

<template>
  <div class="drawer"></div>
  <div class="drawer__block">
    <DrawerHead/>
    <CartItemList/>


    <div class="drawer__block__total">

      <div class="drawer__block__total__line">
        <span>Итого:</span>
        <div class="drawer__block__total__line__space-between"></div>
        <b>12990 ₽</b>
      </div>

      <div class="drawer__block__total__line">
        <span> Налог 5%:</span>
        <div class="drawer__block__total__line__space-between"></div>
        <b>900 ₽</b>
      </div>

      <button
          disabled=""
          class="drawer__block__total__button">
        Оформить заказ
      </button>
    </div>


  </div>
</template>

<style scoped>

</style>