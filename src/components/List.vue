<template>
  <ul class="list container__list">
    <li class="list__item" v-for="pokemon in data" :key="pokemon.name" v-on:click="selectPokemon(pokemon)">
      <button :class="{ active: active === pokemon.name }">{{pokemon.name}}</button>
      
    </li>
    <li v-if="!data.length">
      <p>
        <b>No results found</b> <br>
        Is that a Pokémon in the first Generation?
      </p>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'List',
  props: ['data'],

  data() {
    return {
      selected: 'none',
      active: ''
    }
  },

  methods: {
    selectPokemon: function(event) {
      this.selected = event.url
      this.active = event.name
      this.$emit('iChooseYou', event.url)
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.list {
  margin: 0;
  padding: 0;
  list-style: none;
  max-height: 400px;
  border: 2px solid green;
  overflow: auto;
  border-radius: 14px;
  border: 2px solid var(--secondaryColor);

  &__item {
    &:first-child button{ border-top: none; }
    &:last-child button{ border-bottom: none; }
  }

  @media screen and (min-width: 500px) {
    max-height: 600px;
  }

  button {
    transition: all 0.25s linear;
    margin: 0;
    padding: 10px 20px;
    text-transform: uppercase;
    display: block;
    width: 100%;
    border: none;
    font-size: 16px;
    line-height: 20px;
    border-top: 2px solid transparent;
    border-bottom: 2px solid transparent;
    background: transparent;
    
    &:hover, &:focus{
      cursor: pointer;
      outline: none;
      border-top-color: var(--secondaryColor);
      border-bottom-color: var(--secondaryColor);
    }
  }
}

.active, .active:focus {
  border-color: var(--primaryColor);
  background-color: var(--primaryColor);
}

</style>
