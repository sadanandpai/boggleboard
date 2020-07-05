<template>
  <div id="app">
    <h2><a href="https://leetcode.com/problems/word-search/" target="_new">Boggle Board</a></h2>
    <Board/>
  </div>
</template>

<script>
import Board from './components/Board.vue'

export default {
  name: 'App',
  components: {
    Board
  }
}
</script>

<style>
  #app{
    text-align: center;
  }

  *{
    box-sizing: border-box;
    padding: 0;
  }

  a{
    text-decoration: none;
  }
</style>