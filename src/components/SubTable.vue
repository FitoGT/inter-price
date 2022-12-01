<template>
  <ul class="subtable-table">
    <li v-for="quote, key of filteredQuotes" :key="`quote-${key}`">
      <span v-for="data, key of quote" :key="key">
        <span v-show="showQuoteElements.includes(key)">{{ key }} : {{ data }} </span>
      </span>
    </li>
  </ul>
</template>
<script>
export default {
  name: 'SubTable',
  props: {
    filters: {
      type: Object,
      default: () => { }
    },
    quotes: {
      type: Array,
      default: () => []
    },
  },
  data() {
    return {
      showQuoteElements: ['Amount', 'CouponType', 'Currency', 'Years', '3MLSpread', 'Spread', 'Yield']
    }
  },
  computed: {
    filteredQuotes() {
      let results = this.quotes
      if (this.filters.currency) {
        results = results.filter(result => result.Currency === this.filters.currency)
      }
      if (this.filters.years) {
        results = results.filter(result => this.filters.years.includes(result.Years))
      }
      return results
    }
  },
  watch: {
    filters: {
      deep: true,
      handler: function (filter) {
        if (filter.display) {
          this.showQuoteElements.push(filter.display)
          switch (filter.display) {
            case '3MLSpread':
              this.showQuoteElements = this.showQuoteElements.filter(show => show !== 'Spread' && show !== 'Yield')
              break;
            case 'Yield':
              this.showQuoteElements = this.showQuoteElements.filter(show => show !== 'Spread' && show !== '3MLSpread')
              break;
            case 'Spread':
              this.showQuoteElements = this.showQuoteElements.filter(show => show !== 'Yield' && show !== '3MLSpread')
              break;
          }
        }
      }
    }
  }
}
</script>
<style>
.subtable-table li {
  list-style: none;
  padding: 5px 10px;
  border: 1px solid #eee;
}

.subtable-table li span span:first-child {
  padding: 5px 10px;
  border-right: 1px solid #eee;
}
</style>