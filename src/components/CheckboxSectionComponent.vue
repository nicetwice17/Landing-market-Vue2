<template>
  <div class="option_box">
    <h4>
      {{filterCategory}}
    </h4>
    <div v-for="(item) in filterOptions[filterEndPoint]" v-bind:key="item.id" class="option_item">
      <input  class="custom-checkbox" type="checkbox" :value="item"
             v-bind:id="`format_checkbox${item.id}`" v-model="checkedFilters">
      <label v-bind:for="`format_checkbox${item.id}`">{{ item.name }}</label>
    </div>
  </div>
</template>

<script>

export default {
  name: "CheckboxSectionComponent",
  props: {
    filterCategory: String,
    filterOptions: Object,
    filterEndPoint: String
  },
  mounted() {
    console.log(this.filterEndPoint)
    // console.log()
  }

}
</script>

