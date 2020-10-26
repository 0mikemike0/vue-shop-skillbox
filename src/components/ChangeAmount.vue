<template>
  <div class="product__counter form__counter">
    <button type="button" aria-label="Убрать один товар" @click="decrement">
      <svg width="12" height="12" fill="currentColor">
        <use xlink:href="#icon-minus"></use>
      </svg>
    </button>

    <input type="text" v-model="amount">

    <button type="button" aria-label="Добавить один товар" @click="increment">
      <svg width="12" height="12" fill="currentColor">
        <use xlink:href="#icon-plus"></use>
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  props: ['productAmount'],
  model: {
    prop: 'productAmount',
    event: 'amountChange',
  },
  computed: {
    amount: {
      get() {
        return this.productAmount;
      },
      set(value) {
        this.$emit('amountChange', value);
      },
    },
  },
  methods: {
    increment() {
      this.$emit('amountChange', this.productAmount + 1);
    },
    decrement() {
      if (this.productAmount === 1) return;
      this.$emit('amountChange', this.productAmount - 1);
    },
  },
};
</script>
