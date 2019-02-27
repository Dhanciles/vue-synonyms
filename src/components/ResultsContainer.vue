<template>
  <section class="container">
    <article class="synonym-container" v-for="(synonym, index) in synonymList" :key="index">
      <p class="initial-synonyms" @click="getAdditionalSynonyms({synonym})">{{synonym}}</p>

      <template v-show="selected" v-for="(additionalSynonyms, index) in additionalSynonymList"  class="addtional-synonyms-container">
        <p :key="index" class="secondary-synonyms">{{additionalSynonyms}}</p>
      </template>
    </article>
  </section>
</template>


<script>
import key from '../constants.js'

export default {
  name: 'ResultsContainer', 
  props: {
    synonymList: Array
  }, 
  data: function () {
    return {
      additionalSynonymList: [], 
      selected: false
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

    getAdditionalSynonyms: function (synonym) {
      this.selected = true; 
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${synonym.synonym}?key=${key}`
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
