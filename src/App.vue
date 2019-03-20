<template>
  <div id="app">
    <img src="./assets/logo.png">
    <PokeSearch/>
  </div>
</template>

<script>
import PokeSearch from './components/PokeSearch'

export default {
  name: 'App',
  components: {
    PokeSearch
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
