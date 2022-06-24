<template>
	<b-container class="container">
		<b-row>
			<b-col class="filters_col" cols="3">
        <span class="column_name">
          Фильтры:
        </span>
				<div class="option_box">
          <h4>Формат магазина</h4>
          <div v-for="(item) in filterOptions.format" v-bind:key="item.id" class="option_item">
            <input v-on:click="updateFilters" class="custom-checkbox" type="checkbox" :value="item" v-bind:id="`format_checkbox${item.id}`">
            <label v-bind:for="`format_checkbox${item.id}`">{{ item.name }}</label>
          </div>
				</div>

        <div class="option_box">
          <h4>Категории товара</h4>
            <div v-for="(item) in filterOptions.categories" v-bind:key="item.id" class="option_item">
              <input v-on:click="updateFilters" class="custom-checkbox" type="checkbox" v-bind:value="item" v-bind:id="`categories_checkbox${item.id}`">
              <label v-bind:for="`categories_checkbox${item.id}`">{{ item.name }}</label>
            </div>
        </div>

        <div class="option_box">
          <h4>Тип скидки</h4>
            <div v-for="(item) in filterOptions.sales" v-bind:key="item.id" class="option_item">
              <input v-on:click="updateFilters" class="custom-checkbox" type="checkbox" v-bind:value="item" v-bind:id="`sales_checkbox${item.id}`">
              <label v-bind:for="`sales_checkbox${item.id}`">{{ item.name }}</label>
            </div>
        </div>

			</b-col>
			<b-col cols="9">
         <span class="column_name">
          Найдено акций: {{this.catalog.length}}
        </span>
        <div class="catalog_wrapper row">
          <div
              v-for="(item) in catalog"
              v-bind:title="item.name"
              v-bind:key="item.id"
              class="card col-12 col-sm-6 col-md-3 col-l">
              <h4 class="discountName">
                {{item.discountName}}
              </h4>
            <div class="card_image">
              <img v-bind:src="item.img" alt="">
            </div>
            <p class="card_name">
              {{item.name}}
            </p>
            <div class="date_wrapper">
              <p class="card_date">
                {{item.dateFrom}}
              </p>
              <p class="card_date">
                {{item.dateTo}}
              </p>
            </div>
          </div>
        </div>
      </b-col>
		</b-row>
	</b-container>
</template>

<script>
export default {
	name: 'CatalogContainer',
	props: {
		filterOptions: Object,
		catalog: Array,
	},
  data() {
    return {
      checkedFilters: []
    }
  },
  methods: {
    updateFilters(event) {
        this.$emit('changeFilters', event.target._value)
    }
  },
	mounted() {
	},
}
</script>

<style lang="scss">
.container {
	width: 100%;
}

.category {
	margin: 20px 0;
	font-weight: 700;
}

.column_name {
  display: block;
  padding-bottom: 10px;
  width: 100%;
  border-bottom: 1px solid rgba(0,0,0,0.2);
}

.filters_col {
  .option_box {
    h4 {
      font-size: 18px;
      display: inline-block;
    }
    margin-top: 20px;
    .option_item {
      margin-top: 20px;
      .custom-checkbox {
        position: absolute;
        z-index: -1;
        opacity: 0;
      }

      .custom-checkbox+label {
        display: inline-flex;
        align-items: center;
        user-select: none;
      }

      .custom-checkbox+label::before {
        content: '';
        display: inline-block;
        width: 24px;
        height: 24px;
        flex-shrink: 0;
        flex-grow: 0;
        border: 1px solid #adb5bd;
        border-radius: 0.25em;
        margin-right: 0.5em;
        background-repeat: no-repeat;
        background-position: center center;
        background-size: 50% 50%;
        cursor: pointer;
      }

      .custom-checkbox:checked+label::before {
        border-color: #b70050;
        background-color: #b70050;
        background-image: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3E%3Cpath fill='%23fff' d='M6.564.75l-3.59 3.612-1.538-1.55L0 4.26l2.974 2.99L8 2.193z'/%3E%3C/svg%3E");
      }
    }
  }
}



.catalog_wrapper {
  .card {
    height: auto;
    border: none;
    border-radius: unset;
    border-bottom: 1px solid rgba(0,0,0,.1);
    cursor: pointer;
    transition: .5s;
    padding: 10px;
    margin-bottom: 0;
    .discountName {
      font-weight: 500;
      font-size: 14px;
      line-height: 18px;
      text-align: center;
    }
    .card_image {
      max-height: 200px;
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
    .card_name {
      font-size: 14px;
      line-height: 18px;
      font-weight: 400;
      margin: 0;
    }
    .date_wrapper {
      margin-top: 10px;
      .card_date {
        margin: 0;
        color: rgba(0,0,0,.5);
        font-size: 12px;
        line-height: 15px;
      }
    }

  }
}



</style>
