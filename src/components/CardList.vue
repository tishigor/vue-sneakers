<script setup>
import Card from "@/components/Card.vue";

defineProps({
  items: Array
})
// const onClickAdd = () => {
//   alert('111')
// }
</script>

<template>
  <div class="sneakers__grid">
    <Card
        v-for="item in items"
        :v-key="item.id"
        :image-url="item.imageUrl"
        :title="item.title"
        :price="item.price"
        :is-favorite="false"
        :on-click-add="onClickAdd"
        :is-added="true"
    />
  </div>
</template>