<template>
  <div id="page-wrap">
    <h1>Shopping Cart</h1>
    <CartProductList :cartItems="cartItems" @remove-from-cart="removeFromCart($event)" />
    <h3 id="total-price">Total: ${{ totalPrice }}</h3>
    <button id="checkout-button">Procees to Checkout</button>
  </div>
</template>

<script>
import axios from 'axios';
import CartProductList from '@/components/CartProductList.vue';

export default {
  name: "CartPage",
  components: {
    CartProductList
  },
  data() {
    return {
      cartItems: []
    }
  },
  methods: {
    async removeFromCart(productId) {
      const result = await axios.delete(`/api/users/12345/cart/${productId}`);
      this.cartItems = result.data;
    }
  },
  computed: {
    totalPrice() {
      return this.cartItems.reduce((total, item) => total + Number(item.price), 0);
    }
  },
  async created() {
    const result = await axios.get('/api/users/12345/cart');
    this.cartItems = result.data;
  }
}
</script>

<style scoped>
h1 {
  border-bottom: 1px solid black;
  margin: 0;
  margin-top: 16px;
  padding: 16px;
}

#total-price {
  padding: 16px;
  text-align: right;
}

#checkout-button {
  width: 100%;
}
</style>