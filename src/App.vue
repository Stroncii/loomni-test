<template>
  <div id="app">
    <img src="./assets/logo.png">
    <complete/>
  </div>
</template>

<script>
import complete from './components/complete'

export default {
  name: 'App',
  components: {
    complete
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
