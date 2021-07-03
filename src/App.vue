<template>
  <h1 class="app__header">Simon Says</h1>
  <game-board></game-board>
</template>

<script>

import GameBoard from './components/GameBoard.vue'

export default {
  name: 'App',
  data() {
    return {
      round: 0,
      option: null,
      sequence: null
    }
  },
  components: {
   GameBoard
  }
}
</script>

<style>
@import url('./vendor/normalize.css');

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.app__header{
  margin: 1em 0 0;
}
</style>
