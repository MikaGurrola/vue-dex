<template>
  <div class="home">
    <h1>Pokedex</h1>
    <div class="container">
      <ul class="list">
        <li v-for="pokemon in pokedexEntries.results" :key="pokemon.name" v-on:click="selectPokemon(pokemon)">
          <button class="list__item" :class="{ active: name === pokemon.name }">{{pokemon.name}}</button>
          <!-- <p class="list__item" :class="{ active: name === pokemon.name }">{{pokemon.name}}</p> -->
        </li>
      </ul>
      <ListItem v-bind:pokemon="selected" v-bind:name="name" />
    </div>

  </div>
</template>

<script>
const Pokedex = require('pokeapi-js-wrapper');
const P = new Pokedex.Pokedex();
import ListItem from './ListItem.vue';

export default {
  name: 'Home',
  components: {
    ListItem
  },

  data() {
    return {
      pokedexEntries: {},
      selected: 'none',
      name: 'none'
    }
  },

  mounted: function () {
    localStorage.getItem('pokedex') ? this.pokedexEntries = JSON.parse(localStorage.getItem('pokedex')) : this.getPokedexData();
  },


  methods: {

    getPokedexData: function () {
      const vm = this;
      const interval = {
        limit: 151,
        offset: 0
      }
      P.getPokemonsList(interval)
        .then(function(response) {
          vm.pokedexEntries = response;
          localStorage.setItem('pokedex', JSON.stringify(response));
        }
      )
    },

    selectPokemon: function(event) {
      const vm = this;
      vm.selected = event.url;
      vm.name = event.name
    }

  }


}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;
  align-items: start;
  border: 2px solid blue;
  max-width: 600px;
  margin: auto;
}

.list {
  margin: 0;
  padding: 0;
  list-style: none;
  max-height: 600px;
  border: 2px solid green;
  overflow: auto;

  &__item {
    transition: all 0.25s linear;
    margin: 0;
    padding: 5px 10px;
    margin: 0;
    text-transform: uppercase;
    background-color: white;
    display: block;
    width: 100%;
    border: none;
    font-size: 14px;
    line-height: 20px;
    padding: 10px 20px;
    border: 2px solid transparent;

    &:hover, &:focus{
      cursor: pointer;
      outline: none;
      border-color: var(--secondaryColor);
    }
  }
}

.active, .active:focus {
  border-color: var(--primaryColor);
  background-color: var(--primaryColor);
}

</style>
