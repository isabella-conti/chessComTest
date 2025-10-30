<script setup>
import ChessBoard from './components/ChessBoard/ChessBoard.vue'
</script>

<template>
  <div id="app">
    <h1>Chess.com Board</h1>
    <ChessBoard />
  </div>
</template>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  background-color: #262421;
  min-height: 100vh;
  color: white;
}

h1 {
  margin-bottom: 2rem;
}
</style>
