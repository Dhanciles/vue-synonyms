<template>
  <div id="app">
    <header>
      <form v-on:submit.prevent="searchForSynonyms(search)">
        <input type="text" class="search" name="search" placeholder="Search" v-model="search" />
      </form>
    </header>
    <ResultsContainer :synonymList="synonymList"/>
    <template v-if="synonymList.length">Synonyms For - {{search}}</template>
  </div>
</template>

<script>
import key from './constants.js'
import ResultsContainer from './components/ResultsContainer.vue'

export default {
  name: 'app',
  components: {
    ResultsContainer
  }, 
  data: function () {
    return {
      search: '',
      synonymList: []
    } 
  }, 
  methods: {
    cleaner: function (words) {
      return words.reduce((acc, word) => {
        word.meta.syns.forEach(item => {
          item.forEach(element => {
            acc.push(element)
          })
        })
        return acc
      }, [])
    },

    searchForSynonyms: function (search) {
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${search}?key=${key}`
      fetch(url)
      .then(response => response.json())
      .then(results => this.$data.synonymList = this.cleaner(results))
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
  height: 15vh; 
  width: 100%; 
  background-color: #47B784; 
  display: flex;
  padding: 8px; 
}

input {
  height: 100px; 
  width: 450px; 
  border: none; 
  border-bottom: 3px solid #dee4eb;
  line-height: 1rem; 
  font-size: 3rem; 
  font-weight: 800; 
  color: #dee4eb;  
  background-color: #47B784; 
  margin: 30px 0 0 30px; 
  text-indent: 1rem; 
}

::placeholder {
  font-size: 3rem; 
  font-weight: 800; 
  color: #dee4eb;
}

</style>
