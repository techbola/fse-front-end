<template>
  <div v-if="cartItems.length > 0">
    <CartProductListItem v-for="product in cartItems" :key="product.id" :product="product"
      @remove-from-cart="$emit('remove-from-cart', $event)" />
  </div>
  <p v-else>You have no items in your cart!</p>
</template>

<script>
import CartProductListItem from '@/components/CartProductListItem.vue';

export default {
  name: "CartProductList",
  components: {
    CartProductListItem
  },
  props: {
    cartItems: {
      type: Array,
      required: true
    }
  }
}
</script>