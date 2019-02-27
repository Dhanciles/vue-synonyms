<template>
  <section class="container">
    <article v-for="synonym in synonymList" :key="synonym.id">
      <p @click="getAdditionalSynonyms({synonym})">{{synonym}}</p>
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
      additionalSynonymList: []
    }
  }, 
  methods: {
    getAdditionalSynonyms: function (synonym) {
      console.log(synonym)
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${synonym.synonym}?key=${key}`
      console.log(url)
      fetch(url)
      .then(response => response.json())
      .then(results => this.$data.additionalSynonymList = results.reduce((acc, currentResult) => {
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
  .container {
    background-color: #36495D; 
    width: 100%; 
    height: 80vh; 
    overflow: scroll;
    padding: 8px; 
  }

  article {
    cursor: pointer;
    border-radius: 25px; 
    border: 3px solid #dee4eb;
    margin: 10px 0 10px 0; 
    width: 50%; 
    display: flex; 
    align-items: center; 
    padding: 8px; 
  }

  article:hover {
    background-color: #dee4eb; 
    color: #36495D; 
  }

  article p:hover {
    color: #36495D; 
  }

  p {
    margin: 3px 0 0 10px; 
    color: #ffffff; 
    font-size: 2rem; 
    font-weight: 800;
  }
</style>
