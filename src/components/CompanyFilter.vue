<template>
  <div class="col-4">
    <div class="input-group mb-3">
      <span class="input-group-text" id="inputGroup-sizing-default">Company</span>
      <input type="text" class="form-control" v-model="company">
      <button @click="setFilterData" class="btn btn-outline-secondary" type="button" id="button-addon2">Button</button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'CompanyFilter',
  data() {
    return {
      company: ''
    }
  },
  methods: {
    setFilterData() {
      this.$emit('setFilterData', {
        key: 'company',
        filter: this.company
      })
    }
  }
}
</script>