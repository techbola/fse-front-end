<template>
  <div id="page-wrap" v-if="product">
    <div id="img-wrap">
      <img :src="product.imageUrl" :alt="product.name">
    </div>
    <div id="product-details">
      <h1>{{ product.name }}</h1>
      <h3 id="price">{{ product.price }}</h3>
      <p>Average Rating: {{ product.averageRating }}</p>
      <div v-if="itemAlreadyInCart">
        <button id="add-to-cart" class="grey-button" v-if="!showSuccessMessage">Item is already in cart!</button>
      </div>
      <div v-else>
        <button id="add-to-cart" v-if="!showSuccessMessage" @click="addToCart">Add to Cart</button>
        <button id="add-to-cart" class="green-button" v-if="showSuccessMessage">Successfully added item to
          cart!</button>
      </div>

      <h4>Description</h4>
      <p>{{ product.description }}</p>
    </div>
  </div>
  <div v-else>
    <NotFoundPage />
  </div>
</template>

<script>
import axios from 'axios';
import NotFoundPage from './NotFoundPage.vue';

export default {
  name: "ProductDetailsPage",
  components: {
    NotFoundPage
  },
  data() {
    return {
      product: {},
      showSuccessMessage: false,
      cartItems: [],
    }
  },
  methods: {
    async addToCart() {
      await axios.post('/api/users/12345/cart', {
        productId: this.$route.params.id
      });

      this.showSuccessMessage = true;

      setTimeout(() => {
        this.$router.push('/products');
      }, 1500)
    }
  },
  computed: {
    itemAlreadyInCart() {
      return this.cartItems.some(item => item.id === this.product.id);
    }
  },
  async created() {
    const { data: product } = await axios.get(`/api/products/${this.$route.params.id}`);
    this.product = product;

    const { data: cartItems } = await axios.get('/api/users/12345/cart');
    this.cartItems = cartItems;
  }
}
</script>

<style scoped>
#page-wrap {
  margin-top: 16px;
  padding: 16px;
  max-width: 600px;
}

#img-wrap {
  text-align: center;
}

img {
  width: 400px;
}

#product-details {
  padding: 16px;
  position: relative;
}

#add-to-cart {
  width: 100%;
}

#price {
  position: absolute;
  top: 24px;
  right: 16px;
}

.green-button {
  background-color: green;
}

.grey-button {
  background-color: grey;
}
</style>