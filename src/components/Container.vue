<template>
  <div class="container-fluid">
    <div class="row">
      <quote-filters @setFilterData="setFilterData($event)" key="currency-filter" :filters="getCurrencies"
        type="currency" :input="'radio'" />
      <quote-filters @setFilterData="setFilterData($event)" key="years-filter" :filters="getYears" type="years"
        :input="'checkbox'" />
      <quote-filters @setFilterData="setFilterData($event)" key="display-filter" :filters="display" type="display"
        :input="'radio'" />
    </div>
    <div class="row">
      <company-filter @setFilterData="setFilterData($event)" />
    </div>
    {{ filterItems }}
  </div>
</template>

<script>
import json from '@/assets/data.json'
import QuoteFilters from './QuoteFilters'
import CompanyFilter from './CompanyFilter'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Container',
  components: {
    QuoteFilters,
    CompanyFilter
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
      }
    }
  },
  methods: {
    setFilterData(e) {
      this.filters[e.key] = e.filter
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
            years.push(quote.Years)
          })
        }
      });
      years = [...new Set(years)]
      years = years.sort(function (a, b) { return a - b; });
      return years
    },
    filterItems() {
      let results = this.items
      if (this.filters.company) {
        results = results.filter(i => i.Company.toLowerCase() == this.filters.company.toLowerCase())
      }


      return results

    }
  }
}
</script>