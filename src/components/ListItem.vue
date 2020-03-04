<template>
  <div class="pokemonCard" v-if="data !== 'empty'">
    <h1 class="pokemonCard__name">#{{data.order}} {{name}}</h1>
    <div class="pokemonCard__sprite">
      <img v-bind:src="data.sprites.front_default" :alt="`Front view of ${data.name}`">
      <img v-bind:src="data.sprites.back_default" :alt="`Back view of ${data.name}`">
    </div>
    <ul class="stats">
      <li class="stats__item"> <span class="title">Height</span> {{data.height}}<i>dm</i> </li>
      <li class="stats__item"> <span class="title">Weight</span> {{data.weight}}<i>hg</i> </li>
      <li class="stats__item">
        <span class="title">Types</span> <span class="type" v-for="type in data.types" :key="type.name">{{type.type.name}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ListItem',
  props: ['pokemon', 'name'],
  data: function () {
    return {
      data: 'empty'
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
          return response.json();
        })
        .then((data) => {
          this.data = data;
        });
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
  margin: auto;
  margin: auto;
  display: flex;
  flex-direction: column; 
  align-items: center;
  justify-content: space-around;

  &__name {
    text-transform: uppercase;
    margin: 0;
    font-size: 1.8em;
    @media screen and (min-width: 500px) {
      margin: 1em 0;
      font-size: 2em;
    }
  }
}

.stats {
  margin: 0;
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

</style>
