<template>
  <div class="home">
    <section class="hero">
        <div class="hero-body">
            <p class="title">
                Welcome to Lizardic Shop
            </p>
        </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
          <h2 class="is-size-2 mb-6">Latest items</h2>
      </div>

      <ProductBox
        v-for="item in latestProducts"
        v-bind:key="item.id"
        v-bind:item="item" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import ProductBox from '@/components/ProductBox'

export default {
  name: 'HomeView',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Home | Lizardic Shop'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/latest-items')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>
