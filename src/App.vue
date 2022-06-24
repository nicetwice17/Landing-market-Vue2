<template>
  <div id="app">
    <HeaderComponent pageTitle="Акции и спецпредложения" />

    <CatalogContainer
        :catalog="catalog"
        :filterOptions="filterOptions"
        :SelectFilterOptions="SelectFilterOptions"
        v-on:changeFilters="updateFilters($event)"
        v-on:sortCatalog="sortCatalog($event)"
    />
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
      filterOptions: {},
      filterUrl: '',
      SelectFilterOptions: [
        { value: 'new_increase', text: 'Новизна цена по возрастанию' },
        { value: 'new_descending', text: 'Новизна цена по убыванию' },
        { value: 'cost_increase', text: 'Цена по возрастанию' },
        { value: 'cost_descending', text: 'Цена по убыванию' },
        { value: 'sale_increase', text: 'Скидка по возрастанию' },
        { value: 'sale_descending', text: 'Скидка цена по убыванию' }
      ]
    }
  },
  methods: {
    updateFilters(filter) {
        if(this.filterUrl.indexOf(filter.type) === -1) {
          this.filterUrl += `${filter.filterType}=${filter.type}&`
        } else {
          this.filterUrl = this.filterUrl.replace(`${filter.filterType}=${filter.type}&`, '')
        }

      axios.get(`http://localhost:3000/products?${this.filterUrl}`)
          .then(response => this.catalog = response.data)
    },
    sortCatalog(value){
      console.log(value)
      value === 'cost_increase' && this.catalog.sort((a, b) => a.new - b.new)
      value === 'cost_descending' && this.catalog.sort((a, b) => b.new - a.new);
      value === 'sale_increase' && this.catalog.sort((a, b) => a.discount - b.discount)
      value === 'sale_descending' && this.catalog.sort((a, b) => b.discount - a.discount);
    }

  },
  mounted() {
    Promise.all([
      axios.get('http://localhost:3000/products?_page=1')
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
