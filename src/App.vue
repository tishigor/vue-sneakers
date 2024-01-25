<script setup>
import { ref, watch, provide, computed} from "vue";
import Header from "@/components/Header.vue";
import Drawer from "@/components/Drawer.vue";
import Increment from './components/Increment.vue'

// Корзина (start)
const cart = ref([]);
const drawerOpen = ref(false);
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

const closeDrawer = () => {
  drawerOpen.value = false;
}

const openDrawer = () => {
  drawerOpen.value = true;
}

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1);
  item.isAdded = false;
}

watch(
    cart,
    () => {
      localStorage.setItem('cart', JSON.stringify(cart.value))
    },
    { deep: true }
);

// Объявляю provide в родительских компонентах и далее в дочерних он будет нам доступен. Это нужно для избежания prop drilling
provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
// Корзина (end)
</script>

<template>
  <Drawer
      v-if="drawerOpen"
      :total-price="totalPrice"
      :vat-price="vatPrice"
  />
  <div class="wrapper">
    <Header :total-price="totalPrice" @open-drawer="openDrawer"/>

    <div class="sneakers">
      <router-view></router-view>
    </div>
  </div>


  <Increment/>

</template>

