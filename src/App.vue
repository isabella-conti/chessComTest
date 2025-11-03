<template>
  <div class="container">
    <ChessBoard @square-click="addSquare" />
    <Sidebar :squares="selectedSquares" :position="sidebarPosition" @clear="clearSquares" />
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import ChessBoard from './components/ChessBoard/ChessBoard.vue'
import Sidebar from './components/Sidebar/Sidebar.vue'

const selectedSquares = ref([])
const sidebarPosition = ref('right')
let mq = null
let update = null

const addSquare = (squareName) => {
  selectedSquares.value.push(squareName)
}

const clearSquares = () => {
  selectedSquares.value = []
}

const isIPadLike = () =>
  navigator.platform === 'iPad' ||
  (navigator.userAgent.includes('Mac') && 'ontouchend' in document) ||
  /iPad|iPhone|iPod/.test(navigator.userAgent)

onMounted(() => {
  mq = window.matchMedia('(max-width: 52rem)')
  update = () => {
    sidebarPosition.value = (mq.matches || isIPadLike()) ? 'bottom' : 'right'
  }
  update()
  if (mq.addEventListener) mq.addEventListener('change', update)
  else mq.addListener(update)
})

onBeforeUnmount(() => {
  if (!mq || !update) return
  if (mq.removeEventListener) mq.removeEventListener('change', update)
  else mq.removeListener(update)
})
</script>

<style scoped>
.container {
  display: flex;
  gap: 1.25rem;
  justify-content: center;
  align-items: flex-start;
  flex-wrap: wrap;
  padding: 1.25rem;
}

@media (max-width: 48rem) {
  .container {
    flex-direction: column;
    align-items: center;
  }
}
</style>