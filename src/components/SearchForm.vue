<template>
  <div class="search-form-wrapper">
    <form class="search-form" @submit.prevent="submitForm">
      <div class="form-group" @keydown="errors=[]">
        <input type="text" class="form-control" v-model="query_string" placeholder="Search jokes">
        <div class="errors" v-if="errors.length">
          <div class="error" v-for="e in errors">{{ e }}</div>
        </div>
      </div>
      <button class="btn">Search</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'SearchForm',
  data() {
    return {
      query_string: '',
      errors: []
    }
  },
  methods: {
    submitForm () {
      this.validate()
      if(this.errors.length === 0) {
        this.$emit('search_submit', this.query_string)
      }
    },
    validate () {
      this.errors = []
      if (this.query_string === '') {
        this.errors.push('Search string is required')
      } else if (this.query_string.length < 3) {
        this.errors.push('Search string must be at least 3 characters.')
      }
    }
  }
}
</script>

<style lang="scss">
.search-form{
  display: flex;
  width: 100%;
}
.form-group {
  width: 80%;
  margin-right: 1%;
}
.form-control {
  background: #fff;
  border: 1px solid #d8d8d8;
  font-size: 13px;
  padding: 2%;
  white-space: normal;
  width: 100%;
  height: 37px;
}
.errors {
  padding-top: 5px;
  text-align: left;
  color: red;
  font-size: 12px;
}
</style>
