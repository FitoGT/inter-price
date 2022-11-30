<template>
  <div class="btn-group btn-group-toggle">
    <div v-for="filter, key of filters" :key="`${key}-${type}`">
      <input @change="setFilterData" v-model="selected" :type="input" :name="`${key}-${type}`" :id="`${key}-${type}`"
        autocomplete="off" :value="filter" class="btn-check">
      <label class="btn btn-primary" :for="`${key}-${type}`">{{ filter }}</label>
    </div>
  </div>
</template>
<script>
export default {
  name: 'QuoteFilters',
  props: {
    filters: {
      type: Array,
      default: () => []
    },
    type: {
      type: String,
      default: ""
    },
    input: {
      type: String,
      default: "radio"
    },
  },
  data() {
    return {
      selected: []
    }
  },
  methods: {
    setFilterData() {
      this.$emit('setFilterData', {
        key: this.type,
        filter: this.selected
      })
    }
  }
}
</script>