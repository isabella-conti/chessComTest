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

const rootFont = () => parseFloat(getComputedStyle(document.documentElement).fontSize) || 16

const uiOffsetRem = 6

const adjustByViewport = () => {
  if (!boardRef.value) return

  const availHeight = window.visualViewport?.height ?? window.innerHeight

  const safeBottomCss = getComputedStyle(document.documentElement).getPropertyValue('--safe-area-inset-bottom')
  const safeBottomPx = parseFloat(safeBottomCss) || 0

  const offsetPx = uiOffsetRem * rootFont() + safeBottomPx
  const maxSide = Math.max(0, availHeight - offsetPx)
  boardRef.value.style.maxWidth = `${maxSide}px`
}

const isIOSSafari = () => {
  const ua = navigator.userAgent || ''
  const isiOS = /iP(ad|hone|od)/.test(navigator.platform) || (ua.includes('Mac') && 'ontouchend' in document)
  const isSafari = /Safari/.test(ua) && !/Chrome|CriOS|Chromium|FxiOS|OPiOS|EdgiOS/.test(ua)
  return isiOS && isSafari
}

onMounted(() => {
  ro = new ResizeObserver(entries => {
    const w = entries[0].contentRect.width
    const remValue = w / rootFont()
    document.documentElement.style.setProperty('--board-size-rem', `${remValue}rem`)
  })
  if (boardRef.value) ro.observe(boardRef.value)

  adjustByViewport()
  if (boardRef.value && isIOSSafari()) {
    boardRef.value.classList.add('safari-padding')
  }

  if (window.visualViewport && window.visualViewport.addEventListener) {
    window.visualViewport.addEventListener('resize', adjustByViewport, { passive: true })
  }
  window.addEventListener('resize', adjustByViewport, { passive: true })
  window.addEventListener('orientationchange', adjustByViewport, { passive: true })
})

onBeforeUnmount(() => {
  if (ro && boardRef.value) ro.unobserve(boardRef.value)

  if (boardRef.value && isIOSSafari()) {
    boardRef.value.classList.remove('safari-padding')
  }

  if (window.visualViewport && window.visualViewport.removeEventListener) {
    window.visualViewport.removeEventListener('resize', adjustByViewport)
  }
  window.removeEventListener('resize', adjustByViewport)
  window.removeEventListener('orientationchange', adjustByViewport)
})
</script>

<style scoped>
html,
body {
  height: 100%;
  padding-top: env(safe-area-inset-top, 0px);
  padding-bottom: env(safe-area-inset-bottom, 0px);
  box-sizing: border-box;
}

.chessboard {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  width: clamp(15.625rem, 60vw, 31.25rem);
  max-width: 100%;
  aspect-ratio: 1 / 1;
  border: 0.25rem solid #3c3c3c;
  border-radius: 0.375rem;
  overflow: hidden;
  flex-shrink: 0;
  margin: env(safe-area-inset-top) auto env(safe-area-inset-bottom);
}

.chessboard.safari-padding {
  padding-bottom: calc(env(safe-area-inset-bottom, 0px) + 0.30rem);
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
