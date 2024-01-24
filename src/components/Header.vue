<script setup>
const emit = defineEmits(['openDrawer']);

defineProps({
  totalPrice: Number,
})

</script>

<template>
  <header class="header">

    <div class="header__left-side">
      <img src="/logo.png" alt="Logo" class="logo"/>
      <div>
        <h2 class="header__left-side__sup-title">Vue sneakers</h2>
        <p class="header__left-side__sub-title">Магазин лучших кроссовок</p>
      </div>
    </div>

    <ul class="header__right-side">
      <li @click="() => emit('openDrawer')" class="header__right-side__item">
        <img src="/cart.svg" alt="Cart"/>
        <b>{{ totalPrice }} руб.</b>
      </li>

      <li class="header__right-side__item">
        <img src="/heart.svg" alt="Cart"/>
        <span>Закладки</span>
      </li>
      <li class="header__right-side__item">
        <img src="/profile.svg" alt="Cart"/>
        <span>Профиль</span>
      </li>
    </ul>

  </header>
</template>

<style scoped>

</style>