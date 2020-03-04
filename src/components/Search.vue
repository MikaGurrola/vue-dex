<template>
  <input 
    class="container__search"
    v-model="search"
    @input="searching"
    type="text" 
    name="search" 
    id="search" 
    autocomplete="off" 
    placeholder="Search for 1st Gen pokemon"
  >
</template>

<script>
export default {
  name: 'Search',
  props: ['data'],
  data() {
    return {
      search: ''
    }
  },

  methods: {
    searching() {
      this.$emit('search', this.search)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
input {
  width: 100%;
  padding: 10px 15px;
  border-radius: 12px;
  border: 2px solid var(--secondaryColor);
  display: block;
  box-sizing: border-box;
  font-size: 16px;
  line-height: 20px;
  
  &:focus { 
    outline: none;
    border-color: var(--primaryColor);
  }
}

</style>
