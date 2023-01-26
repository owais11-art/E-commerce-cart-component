<script setup>
  import Menu from './components/Menu.vue'
  import Cart from './components/Cart.vue'
  import { provide, ref, watch } from 'vue'
  import menu from './data/site-data'
  const menuItems = ref(menu)
  const cartItems = ref([])
  provide('cart-items', cartItems)
  const totals = ref({
    subTotal: 0,
    tax: 0,
    grandTotal: 0
  })
  const updateBill = () => {
    totals.value.subTotal = cartItems.value.reduce((total, item) => total += item.total, 0)
    totals.value.tax = totals.value.subTotal * 0.0975
    totals.value.grandTotal = totals.value.subTotal + totals.value.tax
  }
  const addToCart = newItem => {
    const isInCart = cartItems.value.some(item => item.id === newItem.id)
    !isInCart && (cartItems.value = [...cartItems.value, newItem])
    cartItems.value.sort((a, b) => a.id - b.id)
  }
  const removeFromCart = id => {
    cartItems.value = cartItems.value.filter(item => item.id !== id)
  }
  const increaseQuantity = id => {
    cartItems.value = cartItems.value.map(item => {
      if(item.id === id) {
        item.count += 1
        item.total = item.count * item.price
      }
      return item
    })
  }
  const decreaseQuantity = id => {
    cartItems.value = cartItems.value.map(item => {
      if(item.id === id) {
        item.count = item.count > 0 ? item.count - 1 : 0
        item.total = item.count * item.price
      }
      return item
    })
  }
  watch(cartItems, () => {
    updateBill()
  })
</script>

<template>
  <main>
    <Menu 
      :menuItems="menuItems"
      @addToCart="addToCart"
      />
    <Cart 
      :cartItems="cartItems"
      :total="totals"
      @increaseItem="increaseQuantity"
      @decreaseItem="decreaseQuantity"
      @removeFromCart="removeFromCart"
      />
  </main>
</template>

<style scoped lang="less">
  main{
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    padding: 80px 0;
    gap: 20px;
    @media(max-width: 768px){
      flex-direction: column;
      align-items: center;
    }
    div{
      background-color: #fff;
      height: 812px;
      width: 375px;
      border-radius: 6px;
      overflow-y: scroll;
      @media(max-width: 375px){
        width: 100%;
      }
      -ms-overflow-style: none; // hide scroll bar in IE and edge browsers
      scrollbar-width: none; // hide scrollbar in firefox

      // 👇🏻hide scrollbar in Chrome, Safari and Opera browsers
      &::-webkit-scrollbar{
        display: none;
      }
    }
  }
</style>