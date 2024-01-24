<script setup>
const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function,
})
</script>

<template>
  <div class="card">
    <img
        v-if="onClickFavorite"
        @click="onClickFavorite"
        :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
        alt="Like 1"
        class="card__like"
    />
    <img class="card_image" :src="imageUrl" alt="Sneaker"/>

    <p class="card__title">{{ title }}</p>

    <div class="card__bottom">
      <div class="card__bottom__price">
        <span class="card__bottom__price__name">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>
      <img
          v-if="onClickAdd"
          @click="onClickAdd"
          :src="!isAdded ? '/plus.svg' : '/checked.svg'"
          alt="Plus"
      />
    </div>
  </div>
</template>

<style scoped>

</style>