<template>
  <div id="app">
    <div class="header">
      <img alt="Vue logo" src="./assets/logo.png">
      <h1>Pokédex</h1>
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
  img { width: 40px; height: auto; justify-self: end;}
  h1 {width: fit-content; justify-self: start;}
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-gap: 20px;
  align-items: center;
  justify-content: center;
  padding: 0 20px;
  max-width: 900px;
  margin: auto;

  @media screen and (min-width: 600px) {
    grid-template-columns: 40px 1fr;
    padding: 20px;
  }
}

</style>
