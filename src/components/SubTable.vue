<template>
  <ul>
    <li v-for="quote, key of filteredQuotes" :key="`quote-${key}`">
      <span v-for="data, key of quote" :key="key">
        <span>{{ key }} : {{ data }} </span>
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
  }
}
</script>