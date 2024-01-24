<script setup>
import axios from "axios";
import {onMounted, ref} from "vue";
import CardList from "@/components/CardList.vue";

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://5324a7cf8080ae0b.mokky.dev/favorites?_relations=items');
    favorites.value = data.map((obj) => obj.item);
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="sneakers__header__name">Мои закладки</h2>
  <CardList :items="favorites" is-favorites/>
</template>

<style scoped>

</style>