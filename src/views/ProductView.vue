<template>
    <div class="page-item">
        <div class="columns is-multiline">
            <div class="column is-9">
                <figure class="image mb-6">
                    <img v-bind:src="item.get_image">
                </figure>

                <h1 class="title">{{ item.item_name }}</h1>

                <p>{{ item.item_description }}</p>
            </div>

            <div class="column is-3">
                <h2 class="subtitle">Information</h2>

                <p><strong>Price: </strong>₽{{ item.price }}</p>

                <div class="field has-addons mt-6">
                    <div class="control">
                        <input type="number" class="input" min="1" v-model="quantity">
                    </div>

                    <div class="control">
                        <a class="button" @click="addToCart()">Add to cart</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'ProductView',
    data() {
        return {
            item: {},
            quantity: 1
        }
    },
    mounted() {
        this.getProduct()
    },
    methods: {
        async getProduct() {
            this.$store.commit('setIsLoading', true)

            const category_slug = this.$route.params.category_slug
            const item_slug = this.$route.params.item_slug

            await axios
                .get(`/api/v1/items/${category_slug}/${item_slug}`)
                .then(response => {
                    this.item = response.data

                    document.title = this.item.item_name + ' | Lizardic Shop'
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
        addToCart() {
            if (isNaN(this.quantity) || this.quantity < 1) {
                this.quantity = 1
            }

            const item = {
                item: this.item,
                quantity: this.quantity
            }

            this.$store.commit('addToCart', item)

            toast({
                message: 'The item was added to the cart',
                type: 'is-success',
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
            })
        }
    }
}
</script>
