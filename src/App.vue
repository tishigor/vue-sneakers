<script setup>
import { ref, watch, provide, computed} from "vue";
import Increment from './components/Increment.vue'
import Header from "@/components/Header.vue";
import Drawer from "@/components/Drawer.vue";
import Home from "@/pages/Home.vue";

// Корзина (start)
const cart = ref([]);
const isCreatingOrder = ref(false);

const drawerOpen = ref(false);

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)


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

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const {data} = await axios.post(`https://5324a7cf8080ae0b.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value,
    })

    cart.value = [];


    return data;
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false;
  }
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
      @create-order="createOrder"
      :button-disabled="cartButtonDisabled"
  />
  <div class="wrapper">
    <Header :total-price="totalPrice" @open-drawer="openDrawer"/>

    <div class="sneakers">
      <router-view></router-view>
    </div>
  </div>


  <Increment/>

</template>

