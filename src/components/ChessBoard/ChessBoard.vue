<template>
  <div class="chessboard" ref="boardRef">
    <div
      v-for="i in 64"
      :key="i"
      class="square"
      :class="{
        dark: (Math.floor((i - 1) / 8) + ((i - 1) % 8)) % 2 === 1,
        highlight: i - 1 === highlightedSquare
      }"
      @click="handleSquareClick(i - 1)"
    >
      <span v-if="(i - 1) % 8 === 0" class="coord rank">{{ rankNumber(i - 1) }}</span>
      <span v-if="rankNumber(i - 1) === 1" class="coord file">{{ fileLetter(i - 1) }}</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { defineEmits } from 'vue'

const emit = defineEmits(['square-click', 'square-highlight'])
const highlightedSquare = ref(null)
const boardRef = ref(null)
let ro = null

const nameSquare = (index) => {
  const col = String.fromCharCode(65 + (index % 8))
  const row = 8 - Math.floor(index / 8)
  return `${col}${row}`
}

const fileLetter = (index) => String.fromCharCode(97 + (index % 8))
const rankNumber = (index) => 8 - Math.floor(index / 8)

const handleSquareClick = (index) => {
  highlightedSquare.value = index
  const squareName = nameSquare(index)
  emit('square-click', squareName)
  emit('square-highlight', squareName)
}

onMounted(() => {
  const rootFont = () => parseFloat(getComputedStyle(document.documentElement).fontSize) || 16
  ro = new ResizeObserver(entries => {
    const w = entries[0].contentRect.width
    const remValue = w / rootFont()
    document.documentElement.style.setProperty('--board-size-rem', `${remValue}rem`)
  })
  if (boardRef.value) ro.observe(boardRef.value)
})

onBeforeUnmount(() => {
  if (ro && boardRef.value) ro.unobserve(boardRef.value)
})
</script>

<style scoped>
.chessboard {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  width: clamp(15.625rem, 60vw, 31.25rem);
  aspect-ratio: 1 / 1;
  border: 0.25rem solid #3c3c3c;
  border-radius: 0.375rem;
  overflow: hidden;
  flex-shrink: 0;
  margin: 0 auto;
}


.square {
  width: 100%;
  aspect-ratio: 1 / 1;
  cursor: pointer;
  transition: box-shadow 0.2s ease, outline 0.2s ease;
  position: relative;
  user-select: none;
}

.square.dark {
  background-color: #749454;
}

.square:not(.dark) {
  background-color: #ececd4;
}

.square.highlight {
  outline: 0.1875rem solid #63f9bb;
  box-shadow: inset 0 0 0.625rem rgba(0, 255, 85, 0.7);
  z-index: 1;
}

.coord {
  position: absolute;
  font-size: 0.625rem;
  line-height: 1;
  pointer-events: none;
}

.coord.rank {
  left: 0.25rem;
  top: 20%;
  transform: translateY(-50%);
}

.coord.file {
  right: 0.25rem;
  bottom: 0.125rem;
}

.square.dark .coord {
  color: rgba(255, 255, 255, 0.95);
  text-shadow: 0 0.0625rem rgba(0, 0, 0, 0.45);
}

.square:not(.dark) .coord {
  color: rgba(0, 0, 0, 0.75);
  text-shadow: 0 0.0625rem rgba(255, 255, 255, 0.6);
}

</style>