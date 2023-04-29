<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Cart</h1>
            </div>

            <div class="column is-12">
                <table class="table is-fullwidth" v-if="cartTotalLength">
                    <thead>
                        <tr>
                            <th>ProductView</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Total</th>
                            <th></th>
                        </tr>
                    </thead>

                    <tbody>
                        <PositionComp
                            v-for="item in cart.items"
                            v-bind:key="item.id"
                            v-bind:initialItem="item"
                            v-on:removeFromCart="removeFromCart" />
                    </tbody>
                </table>

                <p v-else>You don't have any items in your cart...</p>
            </div>

            <div class="column is-12">
                <h2 class="subtitle">Summary</h2>

                <strong>₽{{ cartTotalPrice.toFixed(2) }}</strong>, {{ cartTotalLength }} items

                <hr>

                <router-link to="/cart/checkout" class="button">Proceed to checkout</router-link>
            </div>
        </div>
    </div>
</template>

<script>
// import axios from 'axios'
import PositionComp from '@/components/PositionComp.vue'

export default {
    name: 'CartView',
    components: {
        PositionComp
    },
    data() {
        return {
            cart: {
                items: []
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
    },
    methods: {
        removeFromCart(item) {
            this.cart.items = this.cart.items.filter(i => i.item.id !== item.id)
        }
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.item.price * curVal.quantity
            }, 0)
        },
    }
}
</script>
