<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <BooksReady/>
</template>

<script>
import BooksReady from './components/BooksReady.vue'

export default {
  name: 'App',
  components: {
    BooksReady
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
