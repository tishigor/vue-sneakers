<script setup>
import Card from "@/components/Card.vue";

defineProps({
  items: Array,
})

const emit = defineEmits(['addToFavorite']);
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
        :on-click-favorite="() => emit('addToFavorite', item)"
        :is-added="item.isAdded"
    />
  </div>
</template>