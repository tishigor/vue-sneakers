<script setup>
import axios from "axios";
import {onMounted, ref} from "vue";
import Increment from './components/Increment.vue'
import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";

const items = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get('https://604781a0efa572c1.mokky.dev/items')
    // console.log(data)
    items.value = data
  }
  catch (err){
    console.log(err)
  }
})

</script>

<template>
  <!--  <Drawer/>-->
  <div class="wrapper">
    <Header/>


    <div class="sneakers">
      <div class="sneakers__header">
        <h2 class="sneakers__header__name">Все кроссовки</h2>

        <div class="sneakers__header__right-side">
          <select class="sneakers__header__right-side__select">
            <option>По названию</option>
            <option>По цене (дешевые)</option>
            <option>По цене (дорогие)</option>
          </select>


          <div class="sneakers__header__right-side__search">
            <img class="sneakers__header__right-side__search__icon" src="/search.svg"/>
            <input
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

