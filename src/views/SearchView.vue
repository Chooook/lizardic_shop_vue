<template>
    <div class="page-search">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Search</h1>

                <h2 class="is-size-5 has-text-grey">Search term: "{{ query }}"</h2>
            </div>

            <ProductBox
                v-for="item in items"
                v-bind:key="item.id"
                v-bind:item="item" />
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox.vue'

export default {
    name: 'SearchView',
    components: {
        ProductBox
    },
    data() {
        return {
            items: [],
            query: ''
        }
    },
    mounted() {
        document.title = 'Search | Lizardic Shop'

        let uri = window.location.search.substring(1)
        let params = new URLSearchParams(uri)

        if (params.get('query')) {
            this.query = params.get('query')

            this.performSearch()
        }
    },
    methods: {
        async performSearch() {
            this.$store.commit('setIsLoading', true)

            await axios
                .post('/api/v1/items/search/', {'query': this.query})
                .then(response => {
                    this.items = response.data
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>
