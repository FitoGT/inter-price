<template>
  <div class="btn-group btn-group-toggle">
    <div v-for="filter, key of filters" :key="`${key}-${type}`">
      <input @change="setFilterData" v-model="selected" :type="input" :name="`${key}-${type}`" autocomplete="off"
        :value="filter">
      {{ filter }}
    </div>
  </div>
  <!-- <div>
    <div v-for="filter, key of filters" :key="`${key}-${type}`">
      <input v-model="selected" type="radio" name="option" autocomplete="off" :value="filter">
      {{ filter }}
    </div>
  </div> -->

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