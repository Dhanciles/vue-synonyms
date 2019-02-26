<template>
  <div id="app">
    <header>
      <form @submit="searchForSynonyms(search)" v-on:submit.prevent="onSubmit">
        <input type="text" class="search" name="search" placeholder="Search" v-model="search"  />
      </form>
    </header>
  </div>
</template>

<script>
import  key   from './constants.js'

export default {
  name: 'app', 
    data: function () {
    return {
      search: '',
      data: []
    } 
  }, 
  methods: {
    searchForSynonyms: function (search) {
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${this.search}?key=${key}`
      console.log(url)
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
  margin-top: 60px;
}
</style>
