<template>
  <section class="container">
    <article v-for="data in data">
      <p>{{data}}</p>
    </article>
  </section>
</template>


<script>
export default {
  name: 'ResultsContainer', 
  props: {
    data: []
  }

}
</script>

<style>
  .container {
    background-color: #36495D; 
    width: 100%; 
    height: 80vh; 
  }

  article {
    cursor: pointer;
  }

  p {
    margin-top: 0; 
    color: #ffffff; 
  }
</style>
