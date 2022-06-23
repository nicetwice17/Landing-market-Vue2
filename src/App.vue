<template>
  <div id="app">
    <HeaderComponent pageTitle="Акции и спецпредложения" />

    <CatalogContainer :catalog="catalog" :filterOptions="filterOptions"
      v-on:changeFilters="updateFilters($event)" />
  </div>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue';
import CatalogContainer from './components/CatalogContainer.vue';
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      catalog: [],
      filteredCatalog: [],
      filterOptions: {},
      filters: [],
      filterUrl: ''
    }
  },
  methods: {
    updateFilters(filter) {

      if(filter.filterType === 'market') {

        if(this.filterUrl.indexOf(filter.type) === -1) {
          this.filterUrl += `marketType=${filter.type}&`
        } else {
          this.filterUrl = this.filterUrl.replace(`marketType=${filter.type}&`, '')
        }
        axios.get(`http://localhost:3000/products?${this.filterUrl}`)
            .then(response => this.catalog = response.data)
      }
    }

  },
  mounted() {
    Promise.all([
      axios.get('http://localhost:3000/products')
        .then(response => (this.catalog = response.data)),
      axios.get('http://localhost:3000/filterOptions')
        .then(response => (this.filterOptions = response.data))
    ])
  },
  components: {
    HeaderComponent,
    CatalogContainer
  }
}
</script>

<style>
#app {
  color: #2c3e50;
  max-width: 1140px;
  margin: 0 auto;
  padding: 20px 0;
}
</style>
