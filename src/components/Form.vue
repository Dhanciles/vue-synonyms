<template>
  <form>
    <input type="text" class="search" name="search" placeholder="Search" v-model="search" v-on:keyup.enter="searchForSynonyms"/>
  </form>
</template>

<script> 
import { key }  from '../constants.js'

export default {
  name: 'Form',
  data: function () {
    return {
      search: '',
      results: []
    } 
  }, 
  methods: {
    searchForSynonyms: function (search) {
      fetch(`https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${this.search}?${key}`)
      .then(response => response.json())
      .then(results => this.results = results.syns)
      .catch(error => console.log(error))
    }
  }
}

</script>
