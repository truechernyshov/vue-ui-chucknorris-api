<template>
  <div id="app">
    <div class="container" v-if="random_jokes.length===3">
      <h1>Chuck Norris Jokes</h1>
      <div class="jokes">
        <h4>Random Jokes</h4>
        <joke v-for="j in random_jokes" :text="j.value"></joke>
      </div>
      <search-form @search_submit="getSearch"></search-form>
      <div class="jokes" v-if="searched_jokes_pagination.length">
        <joke v-for="j in searched_jokes_pagination" :text="j.value"></joke>
        <button class="btn" @click="paginate" :disabled="searched_jokes_pagination.length>=searched_jokes.length">More Results</button>
      </div>
    </div>
  </div>
</template>

<script>
import Joke from './components/Joke.vue'
import SearchForm from '@/components/SearchForm'

export default {
  name: 'App',
  components: {
    Joke, SearchForm
  },
  mounted () {
    this.init()
  },
  data () {
    return {
      loader: null,
      base_url: "https://api.chucknorris.io/jokes/",
      random_jokes: [],
      searched_jokes: [],
      searched_jokes_pagination: [],
      per_page: 5,
      page: 0
    }
  },
  methods: {
    async init () {
      const loader = this.$loading.show()
      while (this.random_jokes.length < 3) {
        const response = await fetch(this.base_url + 'random')
        const joke = await response.json()
        this.random_jokes.push(joke)
      }
      loader.hide()
    },
    async getSearch (query_string) {
      const loader = this.$loading.show()
      this.searched_jokes = []
      this.searched_jokes_pagination = []
      this.page = 0
      const response = await fetch(this.base_url + 'search?query='+query_string)
      const res = await response.json()
      if (res.result !== undefined && res.result.length) {
        this.searched_jokes = res.result
        this.paginate()
      }
      loader.hide()
    },
    paginate () {
      this.page += 1
      const start = this.per_page*(this.page === 1 ? 0 : this.page - 1)
      const items = this.searched_jokes.slice(start, this.per_page+start)
      items.forEach(el => {
        this.searched_jokes_pagination.push(el)
      })
    }
  }
}
</script>

<style lang="scss">
*,*::before, *::after{
  box-sizing: border-box;
}
#app {
  font-family: 16px/1.5  Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}
.btn{
  width: 19%;
  border: none;
  background-color: #f15a24;
  color: #fff;
  cursor: pointer;
  height: 37px;
  &:hover {
    background-color: #e84b0f;
  }
  &:disabled {
    opacity: .5;
    cursor: not-allowed;
  }
}
</style>
