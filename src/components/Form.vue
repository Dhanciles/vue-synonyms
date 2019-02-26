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
      search: ''
    } 
  }, 
  methods: {
    searchForSynonyms: function (search) {
      // console.log(data.search)
    }
  }
}

</script>
