<template>
  <div class="pokemonCard">
    
    <p v-if="data === 'empty'">{{ data }}</p>

    <div v-else>
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
      const vm = this;
      console.log('getting...', vm.name);
      fetch(this.pokemon)
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          vm.data = data;
          console.log(data);
        });

    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.pokemonCard {
  border: 2px solid red;

  &__name {
    text-transform: uppercase;
  }
}

.stats {
  margin: 0;
  list-style: none;
  padding: 0;
  text-align: left;
  max-width: 200px;
  margin: auto;
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
