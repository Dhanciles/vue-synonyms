<template>
  <section class="container">
     <h3 v-if="synonymList.length">Synonyms For - {{currentSearch}}</h3>
    <article class="synonym-container" v-for="(synonym, index) in synonymList" v-bind:key="index">
      <p class="initial-synonyms" @click="getAdditionalSynonyms(synonym, index)">{{synonym}}</p>

      <template  v-for="(additionalSynonyms, i) in additionalSynonymList"  class="addtional-synonyms-container">
        <p :key="i" v-if="index === selectedIndex" class="secondary-synonyms">{{additionalSynonyms}}</p>
      </template>
    </article>
  </section>
</template>


<script>
import apiKey from '../constants.js'

export default {
  name: 'ResultsContainer', 
  props: {
    synonymList: Array, 
    currentSearch: String
  }, 
  data: function () {
    return {
      additionalSynonymList: [], 
      selectedIndex: null
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

    getAdditionalSynonyms: function (synonym, key) {
      console.log(key)
      this.selectedIndex = key
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${synonym}?key=${apiKey}`
      fetch(url)
      .then(response => response.json())
      .then(results => this.$data.additionalSynonymList = this.cleaner(results))
      .catch(error => console.log(error))
    }
  }

}
</script>

<style>
  .container {
    background-color: #36495D; 
    width: 100%; 
    height: 85vh; 
    overflow: scroll;
    padding: 8px; 
  }

  h3 {
    color: #dee4eb;
    font-size: 3rem; 
    font-weight: 800; 
  }

  .synonym-container {
    cursor: pointer;
    border-radius: 25px; 
    border: 3px solid #dee4eb;
    width: 50%; 
    display: flex; 
    align-items: center; 
    padding: 8px; 
    margin: 15px auto; 
    display: flex; 
    flex-direction: column; 
  }

  .synonym-container:hover {
    background-color: #dee4eb; 
    color: #36495D; 
  }

  .synonym-container .initial-synonyms:hover {
    color: #36495D; 
  }

  .initial-synonyms {
    margin: 3px 0 0 10px; 
    color: #ffffff; 
    font-size: 2rem; 
    font-weight: 800;
  }

  .addtional-synonyms-container {
    width: 30%;
    display: flex; 
    flex-flow: wrap; 
    border-radius: 25px; 
    background-color: #dee4eb; 
    padding: 5px; 
  }

  .secondary-synonyms {
    color: #47B784; 
    font-size: 1.5rem; 
    font-weight: 600; 
    margin: 0 5px 0 5px; 
  }
</style>
