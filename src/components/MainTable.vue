<template>
  <div>
    <ul class="main-ul" v-for="item, key of filterItem" :key="key">
      <li>
        <span> {{ item.DateSent }} </span>
        <span> {{ item.Company }} </span>
      </li>
      <div v-if="item.Quote">
        <sub-table :key="`subtable-${item.Company}`" :quotes="item.Quote" :filters="filters" />
      </div>
    </ul>
  </div>

</template>

<script>
import SubTable from './SubTable'
export default {
  components: {
    SubTable
  },
  name: 'MainTable',
  props: {
    filters: {
      type: Object,
      default: () => { }
    },
    items: {
      type: Array,
      default: () => []
    },
  },
  computed: {
    filterItem() {
      let results = this.items
      if (this.filters.company) {
        results = results.filter(i => i.Company.toLowerCase() == this.filters.company.toLowerCase())
      }
      return results
    }
  }
}
</script>