<template>
  <div class="pokemonCard" v-if="data !== 'empty'">
    <h1 class="pokemonCard__name">#{{data.order}} {{name}}</h1>
    <div class="pokemonCard__sprite">
      <img class="back" v-bind:src="data.sprites.back_default" :alt="`Back view of ${data.name}`">
      <img class="front" v-bind:src="data.sprites.front_default" :alt="`Front view of ${data.name}`">
    </div>
    <ul class="stats">
      <li class="stats__item"> <span class="title">Height</span> {{data.height}} </li>
      <li class="stats__item"> <span class="title">Weight</span> {{data.weight}} </li>
      <li class="stats__item">
        <span class="title">Types</span> <span class="type" v-for="type in data.types" :key="type.name">{{type.type.name}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
const Pokedex = require('pokeapi-js-wrapper');
const P = new Pokedex.Pokedex();

export default {
  name: 'ListItem',
  props: ['pokemon', 'name'],
  data: function () {
    return {
      data: 'empty',
      typings: ''
    }
  },

  watch: { 
    pokemon: function() {
      this.fetchData();
    }
  },

  methods: {
    fetchData: function() {
      fetch(this.pokemon)
        .then((response) => {
          return response.json()
        })
        .then((data) => {

          // convert height 
          if(data.height > 3) {
            const heightValue =  data.height * 0.3280839895
            data.height = heightValue.toFixed(1) + " ft"
          } else {
            const heightValue =  data.height * 3.937
            data.height = heightValue.toFixed(1) + " in"
          }

          // convert weight
          const weightValue =  data.weight * 0.220462
          data.weight = weightValue.toFixed(1) + " lbs"


          this.data = data

          this.$emit('passTypes', data.types)

          // TODO: add evolution line
          // this.getEvolution();
        });
    },

    getEvolution: function() {

      P.getEvolutionChainById(this.data.id)
      .then(function(response) {
        console.log(response);
      });
      // fetch(this.pokemon)
      //   .then((response) => {
      //     return response.json();
      //   })
      //   .then((data) => {
      //     console.log('evolution?:', data);
      //   });
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.pokemonCard {
  position: absolute;
  top: 50px;
  right: 0;
  bottom: 50px;
  left: 0;
  height: fit-content;
  margin: auto;
  margin: auto;
  display: flex;
  flex-direction: column; 
  align-items: center;
  justify-content: space-around;

  &__name {
    text-transform: uppercase;
    margin: 30px 0 0 0;
    font-size: 1.8em;
    @media screen and (min-width: 500px) {
      margin: 1em 0;
      font-size: 2em;
    }
  }

  &__sprite {
    width: 208px;
    height: 104px;
    position: relative;
    margin: 30px 0 0 0;
  }
}

.stats {
  margin: 0 0 40px 0;
  list-style: none;
  padding: 0;
  text-align: left;
  max-width: 200px;
}

.title {
  font-weight: bold;
}

.type {
  display: inline-block;
  margin-right: 5px;
  text-transform: capitalize;
}

.front {
  position: absolute;
  top: -45%;
  right: -10%;
}

.back {
  width: 200px;
  position: absolute;
  top: -70%;
  left: -35%;
}

</style>
