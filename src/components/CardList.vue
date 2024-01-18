<script setup>
import {inject} from "vue";
import Card from "@/components/Card.vue";

defineProps({
  items: Array
})
const onClickAdd = () => {
  alert('111')
}

const addToFavorite = inject('addToFavorite')


</script>

<template>
  <div class="sneakers__grid">
    <Card
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :image-url="item.imageUrl"
        :title="item.title"
        :price="item.price"
        :is-favorite="item.isFavorite"
        :on-click-add="onClickAdd"
        :on-click-favorite="() => addToFavorite(item)"
        :is-added="item.isAdded"
    />
  </div>
</template>