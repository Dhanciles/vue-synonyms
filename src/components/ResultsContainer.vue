<template>
  <section class="container">
    <article v-for="data in data">
      <p v-on:click="synonym">{{data}}</p>
    </article>
  </section>
</template>


<script>
import  key   from '../constants.js'

export default {
  name: 'ResultsContainer', 
  props: {
    data: []
  }, 
  data: function () {
    return {
      synonym: '',
      synonymResults: []
    }
  }, 
  methods: {
    getAdditionalSynonyms: function (synonym) {
      let url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${this.synonym}?key=${key}`
      fetch(url)
      .then(response => response.json())
      .then(results => this.$data.synonymResults = results.reduce((acc, currentResult) => {
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
  }

  article {
    cursor: pointer;
  }

  p {
    margin-top: 0; 
    color: #ffffff; 
    font-size: 2rem; 
  }
</style>
