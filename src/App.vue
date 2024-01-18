<script setup>
import axios from "axios";
import {onMounted, ref, reactive, watch, provide} from "vue";
import Increment from './components/Increment.vue'
import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
// todo сделать ожидание полного изменения инпута, и только после этого отправлять запрос
const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
}

// запрашивает список закладок
const fetchFavorites = async () => {
  try {
    const {data: favorites} = await axios.get(`https://5324a7cf8080ae0b.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
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

const addToFavorite = (item) => {
  item.isFavorite = !item.isFavorite

  console.log()
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
    const {data} = await axios.get(`https://604781a0efa572c1.mokky.dev/items`,
        {params})
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false,
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems);

// Объявляю provide в родительских компонентах и далее в дочерних он будет нам доступен. Это нужно для избежания prop drilling
// В данном случае это не нужно, использовал просто для проверки работы provide, inject
provide('addToFavorite', addToFavorite)
</script>

<template>
  <!--  <Drawer/>-->
  <div class="wrapper">
    <Header/>


    <div class="sneakers">
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
        <CardList :items="items"/>
      </div>
    </div>
  </div>
  <Increment/>
</template>

