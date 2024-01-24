<script setup>

const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
})

</script>

<template>

  <div class="drawer__block__cart-item">
    <img class="drawer__block__cart-item__image" :src="imageUrl" :alt="title"/>
    <div class="drawer__block__cart-item__text">
      <p>{{ title }}</p>
      <div class="drawer__block__cart-item__text__price-line">
        <b class="drawer__block__cart-item__text__price-line__price">{{ price }} руб.</b>
        <img
            @click="emit('onClickRemove')"
            class="drawer__block__cart-item__text__price-line__image"
            src="/close.svg"
            alt="Close"/>
      </div>
    </div>
  </div>


</template>

<style scoped>

</style>