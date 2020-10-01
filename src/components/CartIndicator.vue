<template>
  <router-link class="header__cart"  aria-label="Корзина с товарами" :to="{name: 'cart'}">
    <svg width="30" height="21" fill="currentColor">
      <use xlink:href="#icon-cart"></use>
    </svg>
    <span v-if="cartLoading" class="header__count" aria-label="Количество товаров">загружаем...</span>
    <span v-else-if="cartLoadingFailed" class="header__count" aria-label="Количество товаров">ошибка</span>
    <span v-else class="header__count" aria-label="Количество товаров">{{ $store.state.cartProducts.length }}</span>
  </router-link>
</template>

<script>
import { mapActions } from 'vuex';

export default {
  data() {
    return {
      cartLoading: false,
      cartLoadingFailed: false,
    };
  },
  methods: {
    ...mapActions(['loadCart']),
    loadToCart() {
      this.cartLoadingFailed = false;
      this.cartLoading = true;
      this.loadCart()
        .catch(() => { this.cartLoadingFailed = true; })
        .then(() => { this.cartLoading = false; });
    },
  },
  created() {
    this.loadToCart();
  },
};
</script>
