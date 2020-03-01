<template>
  <div class="home">
    <div class="container">
      <div class="container__detail">
        <div class="pokeball" :class="{ rotate: doRotate }">
          <div class="pokeball__center"></div>
          <div class="empty" v-if="selected === 'none'"> empty@</div>
          <ListItem v-bind:pokemon="selected" v-bind:name="name" v-if="selected !== 'none'" />
        </div>
      </div>
      <ul class="list container__list">
        <li v-for="pokemon in pokedexEntries.results" :key="pokemon.name" v-on:click="selectPokemon(pokemon)">
          <button class="list__item" :class="{ active: name === pokemon.name }">{{pokemon.name}}</button>
        </li>
      </ul>
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
      name: 'none',
      doRotate: false
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
      vm.name = event.name;
      vm.doRotate = true
      setTimeout(() => {
        vm.doRotate = false;
      }, 1000);
      
    }

  }


}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.container {
  display: grid;
  grid-template-areas: 
    "detail"
    "list"
  ;
  grid-gap: 20px;
  align-items: start;
  max-width: 900px;
  margin: auto;

  @media screen and (min-width: 500px) {
    grid-template-areas: 
      "list detail"
      "list detail"
      "list detail"
    ; 
    align-items: center;
  }


  &__detail {
    grid-area: detail;
    position: sticky;
    top: 0;
  }

  &__list {
    grid-area: list;
  }
}

.list {
  margin: 0;
  padding: 0;
  list-style: none;
  max-height: 53vh;
  border: 2px solid green;
  overflow: auto;
  border-radius: 14px;
  border: 2px solid var(--secondaryColor);

  @media screen and (min-width: 500px) {
    max-height: 600px;
  }

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

.pokeball {
  height: 325px;
  width: 325px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  border: 2px solid var(--secondaryColor);
  margin: auto;
  transform-origin: center;

  @media screen and (min-width: 500px) {
    height: 350px;
    width: 350px;
  }

  .pokemonCard {
    position: relative; 
    z-index: 1;
  }

  &:before, &:after {
    content: '';
    display: block;
    position: absolute;
    right: 0;
    left: 0;
    height: 160px;
    z-index: -1;

    @media screen and (min-width: 500px) {
      height: 175px;
    }
  }


  &__center {
    height: 70px;
    width: 70px;
    border-radius: 50%;
    background-color: var(--secondaryColor);
    position: absolute;
    z-index: 0;

    @media screen and (min-width: 500px) {
      height: 100px;
      width: 100px;
    }

    &:before {
      content: '';
      display: block;
      position: absolute;
      right: 0;
      left: 0;
      top: 0;
      bottom: 0;
      height: 35px;
      width: 35px;
      border-radius: 50%;
      background-color: white;
      margin: auto;

      @media screen and (min-width: 500px) {
        height: 45px;
        width: 45px;
      }
    }
  }

  &:before {
    /* border: 2px solid blue; */
    background: var(--primaryColor);
    top: 0;
  }
  &:after {
    bottom: 0;
    /* border: 2px solid green; */
    background: white;
  }


  &.rotate {
    /* animation: spin 0.75s cubic-bezier(0.4, 0.0, 0.2, 1) forwards; */
    animation: spin 0.75s linear forwards;
  }
}

@keyframes spin { 
  0% { 
    -webkit-transform: rotate(0deg); 
    transform:rotate(0deg); 
    -webkit-filter: blur(0);
    filter: blur(0);
  } 

  50% {
    -webkit-transform: rotate(180deg); 
    transform:rotate(180deg); 
    -webkit-filter: blur(4px);
    filter: blur(4px);
  }
  100% { 
    -webkit-transform: rotate(360deg); 
    transform:rotate(360deg); 
    -webkit-filter: blur(0);
    filter: blur(0);
  } 
}

</style>
