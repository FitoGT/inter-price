<template>
  <div class="container-fluid">
    <div>
      <h1>{{ msg }}</h1>
      <quote-filters @setFilterData="setFilterData($event)" key="currency-filter" :filters="getCurrencies"
        type="currency" :input="'radio'" />
      <quote-filters @setFilterData="setFilterData($event)" key="years-filter" :filters="getYears" type="years"
        :input="'checkbox'" />
      <quote-filters @setFilterData="setFilterData($event)" key="display-filter" :filters="display" type="display"
        :input="'radio'" />

    </div>
  </div>
</template>

<script>
import json from '@/assets/data.json'
import QuoteFilters from './QuoteFilters'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Container',
  components: {
    QuoteFilters
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
    }
  }
}
</script>