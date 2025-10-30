<template>
  <div class="chessboard-container">
    <div class="chessboard">
      <div
        v-for="i in 64"
        :key="i"
        class="square"
        :class="{ dark: isDark(i - 1) }"
      ></div>
    </div>
  </div>
</template>

<script setup>
const isDark = (index) => {
  const row = Math.floor(index / 8)
  const col = index % 8
  return (row + col) % 2 === 1
}
</script>

<style scoped>
.chessboard-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 2rem;
}

.chessboard {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  aspect-ratio: 1 / 1; 
  max-width: 600px;
  width: 100%;
  border: 4px solid #3c3c3c;
  border-radius: 6px;
  overflow: hidden;
}

.square {
  width: 100%;
  aspect-ratio: 1 / 1;
}

.square.dark {
  background-color: #749454;
}

.square:not(.dark) {
  background-color: #ececd4;
}
</style>
