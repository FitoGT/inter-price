<template>
  <div class="container mt-5">
    <div class="row mb-3">
      <quote-filters @setFilterData="setFilterData($event)" :key="`currency-filter-${componentKey}`"
        :filters="getCurrencies" type="currency" :input="'radio'" />
      <quote-filters v-if="filters.currency" @setFilterData="setFilterData($event)"
        :key="`years-filter-${componentKey}`" :filters="getYears" type="years" :input="'checkbox'" />
      <quote-filters @setFilterData="setFilterData($event)" :key="`display-filter-${componentKey}`" :filters="display"
        type="display" :input="'radio'" />
    </div>
    <div class="row">
      <company-filter @setFilterData="setFilterData($event)" />
    </div>
    <div class="row">
      <main-table :filters="filters" :items="items" />
    </div>
  </div>
</template>

<script>
import json from '@/assets/data.json'
import QuoteFilters from './QuoteFilters'
import CompanyFilter from './CompanyFilter'
import MainTable from './MainTable'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Container',
  components: {
    QuoteFilters,
    CompanyFilter,
    MainTable
  },
  props: {
    msg: String
  },

  data() {
    return {
      items: json.Items,
      display: ['Spread', 'Yield', '3MLSpread'],
      filters: {
        company: null,
        currency: null,
        years: null,
        display: null
      },
      componentKey: 0
    }
  },
  created() {
    this.sortItems()
  },
  methods: {
    setFilterData(e) {
      this.componentKey = +1
      this.filters.years = null
      this.filters.display = null
      this.filters[e.key] = e.filter
    },
    sortItems() {
      this.items = this.items.sort((a, b) => a.Preferred - b.Preferred)
    }
  },
  computed: {
    getCurrencies() {
      let currencies = []
      this.items.forEach(element => {
        let quotes = element.Quote
        if (quotes) {
          quotes.forEach(quote => {
            currencies.push(quote.Currency)
          })
        }
      });
      currencies = [...new Set(currencies)]
      return currencies
    },
    getYears() {
      let years = []
      this.items.forEach(element => {
        let quotes = element.Quote
        if (quotes) {
          quotes.forEach(quote => {
            if (quote.Currency === this.filters.currency) {
              years.push(quote.Years)
            }
          })
        }
      });
      years = [...new Set(years)]
      years = years.sort(function (a, b) { return a - b; });
      return years
    }

  }
}
</script>
<style>

</style>