<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{name: 'main'}">
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Корзина
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Корзина
      </h1>
      <span class="content__info">
      Товаров в корзине: {{ $store.state.cartProducts.length }}
      </span>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST">
        <div class="cart__field">
          <ul class="cart__list">
            <div v-if="cartLoading">Загрузка товаров...</div>
            <div v-if="cartLoadingFailed">Произошла ошибка при загрузке товаров <button @click="loadToCart">Попоробовать ещё раз</button></div>
            <CartItem v-for="item in products" :key="item.productId" :item="item"/>
          </ul>
        </div>

        <div class="cart__block" >
          <p class="cart__desc">
            Мы&nbsp;посчитаем стоимость доставки на&nbsp;следующем этапе
          </p>
          <p class="cart__price">
            Итого: <span> {{ totalPrice | numberFormat }} ₽</span>
          </p>

          <router-link tag="button" :to="{name:'order'}" class="cart__button button button--primery" type="submit" v-if ="$store.state.cartProducts.length">
            Оформить заказ
          </router-link>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import numberFormat from '@/helpers/numberFormat';
import { mapGetters, mapActions } from 'vuex';
import CartItem from '@/components/CartItem.vue';

export default {
  data() {
    return {
      cartLoading: false,
      cartLoadingFailed: false,
    };
  },
  filters: { numberFormat },
  components: {
    CartItem,
  },
  computed: {
    ...mapGetters({
      products: 'cartDetailProducts',
      totalPrice: 'cartTotalPrice',
    }),
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
