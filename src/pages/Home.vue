<script setup>
import {inject, onMounted, reactive, ref, watch} from "vue";
import CardList from "../components/CardList.vue";
import axios from "axios";

const { cart, addToCart, removeFromCart } = inject('cart');

const items = ref([]);


const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

// todo сделать ожидание полного изменения инпута, и только после этого отправлять запрос
const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
}
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      };

      item.isFavorite = true

      const {data} = await axios.post(`https://5324a7cf8080ae0b.mokky.dev/favorites`, obj)

      item.favoriteId = data.id
      console.log(data)
    } else {

      item.isFavorite = false
      await axios.delete(`https://5324a7cf8080ae0b.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }

  console.log()
}

// запрашивает список закладок
const fetchFavorites = async () => {
  try {
    const {data: favorites} = await axios.get(`https://5324a7cf8080ae0b.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!favorite) {
        return item;
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
  } catch (err) {
    console.log(err)
  }
}

// запрашиваем список товаров
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }
    const {data} = await axios.get(`https://5324a7cf8080ae0b.mokky.dev/items`,
        {params})
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }))
  } catch (err) {
    console.log(err)
  }
}


onMounted(async () => {
  // todo Так себе работает. Почистил кэш и первоначальный запрос не выполнится.
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item)=>({
    ...item,
    isAdded: cart.value.some((cartItem)=> cartItem.id === item.id)
  }))
});

// Следим за изменением самого value корзины, а не содержимого корзины. Если хотим следить за содержимым, то пишем deep: true после функции
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
});

watch(filters, fetchItems);
</script>

<template>
  <div class="sneakers__header">
    <h2 class="sneakers__header__name">Все кроссовки</h2>

    <div class="sneakers__header__right-side">
      <select @change="onChangeSelect" class="sneakers__header__right-side__select">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>


      <div class="sneakers__header__right-side__search">
        <img class="sneakers__header__right-side__search__icon" src="/search.svg"/>
        <input
            @change="onChangeSearch"
            class="sneakers__header__right-side__search__input"
            type="text"
            placeholder="Поиск..."
        >
      </div>
    </div>

  </div>

  <div class="sneakers__list">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
  </div>
</template>

<style scoped>

</style>