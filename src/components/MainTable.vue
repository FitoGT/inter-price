<template>
  <div>
    <ul class="main-table" v-for="item, key of filterItem" :key="key">
      <li @click="showSubTable(`subtable-${item.Company}`)"
        :class="show(`subtable-${item.Company}`) ? 'open-li' : 'close-li'">
        <span v-if="item.DateSent"> {{ parseDate(item.DateSent) }} </span>
        <span> {{ item.Company }} </span>
      </li>
      <div v-if="item.Quote">
        <sub-table v-show="show(`subtable-${item.Company}`)" :key="`subtable-${item.Company}`" :quotes="item.Quote"
          :filters="filters" />
      </div>
    </ul>
    <ul class="main-table">
      <li>
        <span> Amount by </span>
        <span v-for="amount of showAmounts" :key="amount" v-show="(!filters.display || amount === filters.display)"> {{
            amount
        }} : {{
    amountCalculation[amount]
}}</span>
      </li>
    </ul>
  </div>

</template>

<script>
import moment from 'moment';
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
  data() {
    return {
      showElements: [],
      showAmounts: ['3MLSpread', 'Spread', 'Yield']
    }
  },
  computed: {
    filterItem() {
      let results = this.items
      if (this.filters.company) {
        results = results.filter(i => i.Company.toLowerCase() == this.filters.company.toLowerCase())
      }
      return results
    },
    amountCalculation() {
      let quoteArray = this.filterItem.map(i => i.Quote).filter(i => i).flat()
      let amounts = {
        "Spread": 0,
        "Yield": 0,
        "3MLSpread": 0
      }
      for (const key in amounts) {
        quoteArray.forEach(q => {
          amounts[key] += q[key]
        })
      }
      return amounts
    }
  },
  methods: {
    parseDate(date) {
      return moment(date).format("DD-MMM-YYYY")
    },
    showSubTable(key) {
      if (this.showElements.includes(key)) {
        const index = this.showElements.indexOf(key);
        if (index > -1) {
          this.showElements.splice(index, 1);
        }
      } else {
        this.showElements.push(key)

      }
    },
    show(key) {
      return this.showElements.includes(key)
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

.main-table .close-li::before {
  content: '▲';
  left: 0;
}

.open-li::before {
  content: '▼';
  left: 0;
}
</style>