<template>
  <div class="home">
    <div class="container">
      <div class="container__detail">
        <div class="pokeball" :class="[typeColors, { rotate: doRotate}]" >
          <div class="pokeball__center">
            <div class="inner"></div>
          </div>
        </div>
        <ListItem :pokemon="selected" v-on:passTypes="updateType" />
      </div>
      <Search :data="pokemans" v-on:search="filterSearch" />
      <List :data="pokemans" v-on:iChooseYou="selectedPokemon" ref="child" />
    </div>
  </div>
</template>

<script>
const Pokedex = require('pokeapi-js-wrapper')
const P = new Pokedex.Pokedex()
import List from './List.vue'
import ListItem from './ListItem.vue'
import Search from './Search.vue'

export default {
  name: 'Home',
  components: {
    List,
    ListItem,
    Search
  },

  data() {
    return {
      pokedexEntries: {},
      pokemans: [],
      selected: 'none',
      typeColors: ['first--none', 'second--none'],
      doRotate: false
    }
  },

  mounted: function () {
    if(localStorage.getItem('pokedex')) {
      this.pokedexEntries = JSON.parse(localStorage.getItem('pokedex'))
      this.pokemans = [...this.pokedexEntries.results]
    } else {
      this.getPokedexData()
    }
  },


  methods: {

    getPokedexData: function () {
      const interval = {
        limit: 151,
        offset: 0
      }
      P.getPokemonsList(interval)
        .then(response => {
          this.pokedexEntries = response
          this.pokemans = [...response.results]
          localStorage.setItem('pokedex', JSON.stringify(response))
        }
      )
    },

    selectedPokemon (value) {
      this.selected = value
      this.doRotate = true
      setTimeout(() => {
        this.doRotate = false
      }, 700)
    },

    updateType (types) {
      const pokemonType = types.length > 1 ? ['first--'+types[0].type.name, 'second--'+types[1].type.name] : ['first--'+types[0].type.name, 'second--none']
      this.typeColors = [...pokemonType]
    }, 

    filterSearch(value) {
      this.pokemans = value.length ? this.pokedexEntries.results.filter(pokemon => pokemon.name.toLowerCase().indexOf(value.toLowerCase()) > -1) : [...this.pokedexEntries.results]
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
    "search"
    "list"
  ;
  grid-gap: 20px;
  grid-template-rows: auto  auto 1fr;
  align-items: start;
  max-width: 900px;
  max-height: 80vh;
  margin: auto;

  @media screen and (min-width: 600px) {
    grid-template-rows:  auto 1fr;
    grid-template-columns: 1fr 1fr;
    grid-template-areas: 
      "search detail"
      "list detail"
    ;
    max-height: initial;
  }


  &__detail {
    grid-area: detail;
    position: relative;
  }

  &__search {
    grid-area: search;
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

  @media screen and (min-width: 600px) {
    height: 350px;
    width: 350px;
  }

  .pokemonCard {
    position: relative; 
    z-index: 1;
  }

  &:before, &:after {
    transition: background-color 0.7s ease-in;
    content: '';
    display: block;
    position: absolute;
    right: 0;
    left: 0;
    height: 140px;
    z-index: -1;

    @media screen and (min-width: 600px) {
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

      @media screen and (min-width: 600px) {
        height: 100px;
        width: 100px;
      }
    }

    &:after {
      background-color: white;
      height: 50px;
      width: 50px;
      @media screen and (min-width: 600px) {
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
      @media screen and (min-width: 600px) {
        height: 45px;
        width: 45px;
      }
    }
  }

  &:before {
    background: var(--primaryColor);
    top: 0;


  }

  &.first {
    &--bug:before {
      background: #9BBF49;
    }
    &--dragon:before {
      background: #2D6EBE;
    }
    &--electric:before {
      background: #EED25A;
    }
    &--fairy:before {
      background: #DF95E1;
    }
    &--fighting:before {
      background: #BF4C6A;
    }
    &--fire:before {
      background: #F2A062;
    }
    &--flying:before {
      background: #93A8D9;
    }
    &--ghost:before {
      background: #566AA7;
    }
    &--grass:before {
      background: #79B866;
    }
    &--ground:before {
      background: #CD7C51;
    }
    &--ice:before {
      background: #89CCC0;
    }
    &--normal:before {
      background: #9299A0;
    }
    &--poison:before {
      background: #A26FC3;
    }
    &--psychic:before {
      background: #E9797A;
    }
    &--rock:before {
      background: #C5B790;
    }
    &--water:before {
      background: #5C90D0;
    }
  }

  &.second {
    &--bug:after {
      background: #9BBF49;
    }
    &--dragon:after {
      background: #2D6EBE;
    }
    &--electric:after {
      background: #EED25A;
    }
    &--fighting:after {
      background: #BF4C6A;
    }
    &--fire:after {
      background: #F2A062;
    }
    &--flying:after {
      background: #93A8D9;
    }
    &--ghost:after {
      background: #566AA7;
    }
    &--grass:after {
      background: #79B866;
    }
    &--ground:after {
      background: #CD7C51;
    }
    &--ice:after {
      background: #89CCC0;
    }
    &--normal:after {
      background: #9299A0;
    }
    &--poison:after {
      background: #A26FC3;
    }
    &--psychic:after {
      background: #E9797A;
    }
    &--rock:after {
      background: #C5B790;
    }
    &--water:after {
      background: #5C90D0;
    }
  }

  &:after {
    bottom: 0;
    background: white;
  }


  &.rotate {
    animation: spin 0.75s linear forwards;
  }
}

@keyframes spin { 
  0% { 
    -webkit-transform: rotate(0deg); 
    transform: rotate(0deg); 
  } 

  50% {
    -webkit-transform: rotate(180deg); 
    transform: rotate(180deg); 
  }
  100% { 
    -webkit-transform: rotate(360deg); 
    transform: rotate(360deg); 
  } 
}


</style>
