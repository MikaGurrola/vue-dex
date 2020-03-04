<template>
  <div class="home">
    <div class="container">
      <div class="container__detail">
        <div class="pokeball" :class="{ rotate: doRotate }">
          <div class="pokeball__center">
            <div class="inner"></div>
          </div>
        </div>
        <ListItem :pokemon="selected" />
      </div>
      <List :data="pokedexEntries.results" v-on:iChooseYou="selectedPokemon" />
    </div>

  </div>
</template>

<script>
const Pokedex = require('pokeapi-js-wrapper');
const P = new Pokedex.Pokedex();
import List from './List.vue';
import ListItem from './ListItem.vue';

export default {
  name: 'Home',
  components: {
    List,
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
      const interval = {
        limit: 151,
        offset: 0
      }
      P.getPokemonsList(interval)
        .then(function(response) {
          this.pokedexEntries = response;
          localStorage.setItem('pokedex', JSON.stringify(response));
        }
      )
    },

    selectedPokemon (value) {
      this.selected = value;
      this.doRotate = true;
      setTimeout(() => {
        this.doRotate = false;
      }, 700);

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
    position: relative;
    /* position: sticky;
    top: 0; */
  }

  &__list {
    grid-area: list;
  }
}

.pokeball {
  height: 280px;
  width: 280px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  border: 2px solid var(--secondaryColor);
  margin: auto;
  transform-origin: center;
  opacity: 0.5;

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
    height: 140px;
    z-index: -1;

    @media screen and (min-width: 500px) {
      height: 175px;
    }
  }


  &__center {
    width: 100%;
    height: 20px;
    background-color: var(--secondaryColor);
    position: absolute;
    z-index: 0;

    &:before, &:after, .inner {
      content: '';
      display: block;
      position: absolute;
      right: 0;
      left: 0;
      top: 0;
      bottom: 0;
      height: 70px;
      width: 70px;
      border-radius: 50%;
      background-color: var(--secondaryColor);
      margin: auto;

      @media screen and (min-width: 500px) {
        height: 100px;
        width: 100px;
      }
    }

    &:after {
      background-color: white;
      height: 50px;
      width: 50px;
      @media screen and (min-width: 500px) {
        height: 65px;
        width: 65px;
      }
    }

    .inner {
      width: 35px;
      height: 35px;
      box-shadow: 0px 0px 2px 0px rgba(0,0,0,0.75);
      z-index: 1;
      background-color: white;
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
    animation: spin 0.75s linear forwards;
  }
}

@keyframes spin { 
  0% { 
    -webkit-transform: rotate(0deg); 
    transform:rotate(0deg); 
  } 

  50% {
    -webkit-transform: rotate(180deg); 
    transform:rotate(180deg); 
  }
  100% { 
    -webkit-transform: rotate(360deg); 
    transform:rotate(360deg); 
  } 
}

</style>
