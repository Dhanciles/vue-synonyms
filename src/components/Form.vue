<template>
  <form>
    <input type="text" class="search" name="search" placeholder="Search" v-model="search"/>
  </form>
</template>

<script> 
export default {
  name: 'Form',
  data: function () {
    return {
      search: ''
    } 
  }
}

</script>
