<template>
  <div class="List">
    <ul class="list container__list">
      <li v-for="pokemon in data" :key="pokemon.name" v-on:click="selectPokemon(pokemon)">
        <button class="list__item" :class="{ active: active === pokemon.name }">{{pokemon.name}}</button>
      </li>
    </ul>
  </div>
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
    border-top: 2px solid transparent;
    border-bottom: 2px solid transparent;

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
