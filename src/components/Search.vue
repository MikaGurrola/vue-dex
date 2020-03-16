<template>
  <div class="input-group">
    <input 
      class="container__search"
      v-model="search"
      @input="searching"
      type="text" 
      name="search" 
      id="search" 
      autocomplete="off" 
      placeholder="Search for a Pokémon"
    >

    <label for="search">
      <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 0 24 24" width="24">
        <path d="M0 0h24v24H0V0z" fill="none"/>
        <path fill="#2C3E50" d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/>
      </svg>
    </label>

  </div>
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
label, input, .input-group:after {
  border: 2px solid var(--secondaryColor);
  border-radius: 22px;
}

.input-group {
  position: relative;
  display: flex;
  &:after {
    content: '';
    display: block;
    border-left: none;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    padding: 8px 20px 8px 0;
    order: 3;
  }
}



svg {
  display: block;
  transform-origin: center left;
  transform: translateX(6px);
  transition: transform 0.25s linear;
}
label {
  display: block;
  width: fit-content;
  padding-right: 0;

  border-right: none;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  padding: 8px 0 8px 8px;
  order: 1;
}

input {
  width: 100%;
  margin: 0;
  border-left: none;
  border-right: none;
  border-radius: 0;
  padding: 0;
  appearance: none;
  display: block;
  box-sizing: border-box;
  font-size: 16px;
  line-height: 20px;
  transition: flex 0.25s ease-out;
  flex: 0;
  order: 2;

  &:focus { 
    outline: none;
    flex: 1;
    padding: 8px;

    &+label svg{
      transform: translateX(0);
    }
  }
}

</style>
