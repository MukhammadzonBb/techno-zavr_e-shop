<template>
  <!-- eslint-disable vuejs-accessibility/form-control-has-label  -->
  <div>
    <!-- eslint-disable-next-line vue/valid-v-else -->
    <main class="content container">
      <div class="content__top">
        <ul class="breadcrumbs">
          <li class="breadcrumbs__item">
            <router-link class="breadcrumbs__link" :to="{ name: 'main' }"> Каталог </router-link>
          </li>
          <li class="breadcrumbs__item">
            <a class="breadcrumbs__link"> Корзина </a>
          </li>
        </ul>

        <h1 class="content__title">Корзина</h1>
        <span class="content__info" v-show="products.length">
          Количество товаров: {{ products.length }}
        </span>
      </div>

      <section class="cart">
        <form class="cart__form form" action="#" method="POST">
          <div class="cart__field" style="position: relative">
            <pulse-loader v-if="isCartLoading" style="transform: scale(0.6)" />
            <ul class="cart__list" v-else>
              <CartItem v-for="item in products" :key="item.productId" :item="item" />
            </ul>
          </div>

          <div class="cart__block">
            <p class="cart__desc">Мы&nbsp;посчитаем стоимость доставки на&nbsp;следующем этапе</p>
            <p class="cart__price" v-show="products.length">
              Итого: <span>{{ totalPrice | numberFormat }} ₽</span>
            </p>

            <router-link
              tag="button"
              :to="{ name: 'order' }"
              class="cart__button button button--primery"
              type="submit"
              :disabled="!products.length"
            >
              Оформить заказ
            </router-link>
          </div>
        </form>
      </section>
    </main>
  </div>
</template>

<script>
/* eslint-disable import/no-extraneous-dependencies */
/* eslint-disable quotes */
import numberFormat from "@/helpers/numberFormat";
import { mapGetters } from "vuex";
// eslint-disable-next-line import/no-unresolved
import CartItem from "@/components/CartItem.vue";
import PulseLoader from "@/components/PulseLoader.vue";

export default {
  filters: {
    numberFormat,
  },
  components: { CartItem, PulseLoader },
  computed: {
    isCartLoading() {
      return this.$store.state.isCartLoading;
    },
    ...mapGetters({
      products: "cartDetailProducts",
      totalPrice: "cartTotalPrice",
    }),
  },
};
</script>
