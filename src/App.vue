<template>
  <div id="app">
    <div class="header">
      <img alt="Vue logo" src="./assets/logo.png">
      <h1>Pokedex</h1>
    </div>
    <Home />
  </div>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;

  --primaryColor: #42b983;
  --secondaryColor: #2c3e50;

  @media screen and (min-width: 501px) {
    margin-top: 60px;
  }
}

.header {
  img { width: 100%; height: auto;}
  display: grid;
  grid-template-columns: 40px 1fr;
  grid-gap: 20px;
  align-items: center;
  justify-content: center;
  padding: 20px;
  h1 {width: fit-content; }
}

</style>
