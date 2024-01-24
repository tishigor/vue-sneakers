<script setup>
import Card from "@/components/Card.vue";

defineProps({
  items: Array,
})

const emit = defineEmits(['addToFavorite', 'addToCart']);
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
        :on-click-favorite="() => emit('addToFavorite', item)"
        :on-click-add="() => emit('addToCart', item)"
        :is-favorite="item.isFavorite"
        :is-added="item.isAdded"
    />
  </div>
</template>