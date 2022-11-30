<template>
  <div>
    <ul class="main-table" v-for="item, key of filterItem" :key="key">
      <li>
        <span v-if="item.DateSent"> {{ item.DateSent }} </span>
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
<style scoped>
.main-table li {
  list-style: none;
  padding: 5px 10px;
  border: 1px solid #eee;
}

.main-table li span:first-child {
  padding: 5px 10px;
  border-right: 1px solid #eee;
}
</style>