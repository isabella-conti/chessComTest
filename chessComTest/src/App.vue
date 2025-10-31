<template>
  <div class="container">
    <ChessBoard @square-click="addSquare" />
    <Sidebar :squares="selectedSquares" @clear="clearSquares" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ChessBoard from './components/ChessBoard/ChessBoard.vue'
import Sidebar from './components/Sidebar/Sidebar.vue'

const selectedSquares = ref([])

const addSquare = (squareName) => {
  selectedSquares.value.push(squareName)
}

const clearSquares = () => {
  selectedSquares.value = []
}
</script>

<style scoped>
.container {
  display: flex;
  gap: 20px;
  justify-content: center;
  align-items: flex-start;
  flex-wrap: wrap;
  padding: 20px;
}

@media (max-width: 600px) {
  .container {
    flex-direction: column;
    align-items: center;
  }
}
</style>
