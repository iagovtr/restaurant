<template>
  <div class="cart">
      <router-link to="/" class="cart--go-back">←️ Voltar</router-link>
      <h2 class="cart--title">Seu pedido</h2>
      <p v-if="hasNoItems">Seu carrinho ainda está vazio.</p>
      <transition-group name="list">
        <CartItem v-for="item in cartList" :key="item.id" :item="item"/>
      </transition-group>
      <div class="cart--total" v-if="!hasNoItems">
          <span>Total: </span>
          <span class="price">{{getCartTotal | currency}}</span>
      </div>
      <button @click="showModal = true">open modal</button>
      <Modal :show="showModal" @on-modal-close="showModal = false">iepa</Modal>
  </div>
</template>

<script>
import CartItem from './CartItem';
import Modal from './Modal.vue';
import {mapGetters} from 'vuex';
import Mixin from '@/mixins/mixins';

export default {
    name: 'Cart',
    mixins: [Mixin],
    components: {
        CartItem,
        Modal
    },
    filters: {
        currency(value) {
            return `R$ ${value.toLocaleString("pt-br", { minimumFractionDigits: 2,})}`;
        }
    },
    data() {
        return {
            showModal: false
        }
    },      
    computed: {
        ...mapGetters([
            'getCartTotal'
        ]),
        cartList () {
            return this.$store.state.cartList;
        },
        hasNoItems() {
            return !this.cartList.length;
        },
    }  
}
</script>

<style lang="less" scoped>
    .cart {
        background: white;
        width: 643px;
        min-width: 643px;
        padding: 50px;

        &--go-back {
            font-weight: 600;
            font-size: 18px;
            text-decoration: none;
            color: black;
            display: none;
        }

        &--title {
            margin-top: 50px;
            font-weight: 600;
            font-size: 24px;
        }

        &--total {
            font-weight: 600;
            font-size: 18px;
            text-align: right;
            margin-top: 30px;

            .price {
                color: @yellow;
                padding-left: 10px;
            }
        }

        @media @tablets {
            width: 100%;
            min-width: unset;
            padding: 50px 20px 20px;

            &--go-back {
                display: block;
            }
        }

        .list-enter-active, .list-leave-active {
            transition: all 1s;
        }
        .list-enter, .list-leave-to {
            opacity: 0;
            transform: translateX(-30px);
        }
    }
</style>