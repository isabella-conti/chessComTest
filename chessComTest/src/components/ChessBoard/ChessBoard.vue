<template>
  <div class="chessboard">
    <div
      v-for="i in 64"
      :key="i"
      class="square"
      :class="{ dark: (Math.floor((i - 1) / 8) + ((i - 1) % 8)) % 2 === 1 }"
      @click="handleSquareClick(i - 1)"
    ></div>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['square-click'])

const nameSquare = (index) => {
  const col = String.fromCharCode(65 + (index % 8))
  const row = 8 - Math.floor(index / 8)
  return `${col}${row}`
}

const handleSquareClick = (index) => {
  const squareName = nameSquare(index)
  emit('square-click', squareName)
}
</script>

<style scoped>
.chessboard {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  width: clamp(250px, 60vw, 500px);
  aspect-ratio: 1 / 1;
  border: 4px solid #3c3c3c;
  border-radius: 6px;
  overflow: hidden;
  flex-shrink: 0;
}

.square {
  width: 100%;
  aspect-ratio: 1 / 1;
  cursor: pointer;
}

.square.dark {
  background-color: #749454;
}

.square:not(.dark) {
  background-color: #ececd4;
}
</style>
