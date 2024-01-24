<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String,
})
</script>

<template>
  <div class="info-block">
    <img :src="imageUrl" alt="Info image" height="80" width="80"/>
    <h2 class="info-block__header">{{ title }}</h2>
    <p class="info-block__paragraph">{{ description }}</p>
  </div>
</template>

<style scoped>

</style>