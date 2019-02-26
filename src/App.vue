<template>
  <div id="app">
    <header>
      <form @submit="searchForSynonyms(search)" v-on:submit.prevent="onSubmit">
        <input type="text" class="search" name="search" placeholder="Search" v-model="search"  />
      </form>
    </header>
    <ResultsContainer :data="data"/>
  </div>
</template>

<script>
import  key   from './constants.js'
import ResultsContainer from './components/ResultsContainer.vue'

export default {
  name: 'app',
  components: {
    ResultsContainer
  }, 
  data: function () {
    return {
      search: '',
      data: []
    } 
  }, 
  methods: {
    searchForSynonyms: function (search) {
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${this.search}?key=${key}`
      fetch(url)
      .then(response => response.json())
      .then(results => this.$data.data = results.reduce((acc, currentResult) => {
        currentResult.meta.syns.forEach(item => {
          item.forEach(element => {
            acc.push(element)
          })
        })
        return acc
      }, []))
      .catch(error => console.log(error))
      this.search = ''
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh; 
}

header {
  height: 20vh; 
  width: 100%; 
  background-color: #47B784; 
}
</style>
