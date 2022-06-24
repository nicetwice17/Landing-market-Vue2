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
			<b-col cols="9" class="catalog_list">
        <div class="filters_list">
             <span class="column_name">
          Найдено акций: {{this.catalog.length}}
        </span>
          <div>
            <select @change="sortCatalog">
              <option v-for="(item) in SelectFilterOptions" v-bind:value="item.value" v-bind:key="item.text">{{item.text}}</option>
            </select>
          </div>
        </div>
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
              <span class="discountPercent">
              {{'-' + item.discount + '%'}}
              </span>
              <span class="oldPrice">
                {{item.old}}
              </span>
              <span class="newPrice">
                {{item.new}}
              </span>
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
        <b-pagination
            v-on:change="changePage"
            v-model="currenPage"
            :total-rows="rows"
            :per-page="perPage"
            last-number
            first-number
            prev-class="arrow"
            next-class="arrow"
            class="pagination"
        ></b-pagination>
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
    SelectFilterOptions: Array,
    perPage: Number,
    pageTotal: Number,
	},
  data() {
    return {
      currenPage: 1
    }
  },
  methods: {
    updateFilters(event) {
        this.$emit('changeFilters', event.target._value)
    },
    sortCatalog(event) {
      this.$emit('sortCatalog', event.target.value)
    },
    changePage(value) {
      this.$emit('changePage', value)
    }
  },
  computed: {
    rows() {
      return this.pageTotal
    }
  }
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



.catalog_list {
  .filters_list {
    display: flex;
    border-bottom: 1px solid rgba(0,0,0,0.2);
    select {
      border: none;
      background-color: #ffffff;
      outline: none;
    }
    .column_name {
      border-bottom: unset;
    }
  }
  .catalog_wrapper {
    .card {
      height: auto;
      border: none;
      border-radius: unset;

      cursor: pointer;
      transition: .5s;
      padding: 16px 24px 16px;
      margin-bottom: 0;
      overflow: hidden;
      border: 1px solid #ffffff;
      border-bottom: 1px solid rgba(0,0,0,.1);
      &:hover {
        transition: .5s;
        border-color: rgba(0,0,0,.1);
      }

      .discountName {
        font-weight: 500;
        font-size: 14px;
        line-height: 18px;
        text-align: center;
      }
      .card_image {
        max-height: 200px;
        margin-top: 20px;
        position: relative;
        img {
          width: 100%;
          height: 100%;
          object-fit: cover;
          &:hover {
            transform: scale(1.05);
            transition: .5s;
          }
        }
        .discountPercent {
          position: absolute;
          top: 0;
          left: -16px;
          width: inherit;
          border-bottom-right-radius: 12px;
          border-top-left-radius: 12px;
          font-size: 14px;
          background-color: #b70050;
          padding: 0 8px;
          line-height: 24px;
          color: #f0f0f0;
        }

        .newPrice {
          position: absolute;
          bottom: 0;
          right: -20px;
          width: inherit;
          border-bottom-right-radius: 25px;
          border-top-left-radius: 25px;
          font-size: 32px;
          font-weight: 600;
          background-color: #ffd500;
          padding: 0 16px;
          line-height: 40px;
          color: #000000;
        }

        .oldPrice {
          position: absolute;
          bottom: 40px;
          right: -20px;
          font-size: 20px;
          padding: 0 4px;
          background-color: #fff;
          color: #000000;
          &:before {
            content: "";
            position: absolute;
            top: 50%;
            left: 0;
            right: 0;
            border-bottom: 1px solid #b70050;

          }
        }
      }
      .card_name {
        font-size: 14px;
        line-height: 18px;
        font-weight: 400;
        margin: 0;
        margin-top: 10px;

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
  .pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
    .page-link {
      border: none;
      color: #000000;
    }
    .page-item.active {
      button {
        background-color: #b70050;
        border-color: #b70050;
      }
    }

    .arrow {
      font-size: 30px;
      line-height: 18px;
      padding-bottom: 6px;
      margin: 0 20px;
      outline: none;
    }
  }
}



</style>
