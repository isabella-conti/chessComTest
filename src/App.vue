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


onMounted(() => {
  mq = window.matchMedia('(max-width: 56rem)')
  update = (e) => {
    sidebarPosition.value = e.matches ? 'bottom' : 'right'
  }
  update(mq)
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
  align-items: flex-start;
  justify-content: center;
  flex-wrap: wrap;
  padding: 5rem 1.25rem 1.25rem;
  max-width: 100%;
  box-sizing: border-box;
}

@media (max-width: 56rem) {
  .container {
    flex-direction: column;
    align-items: center;
    padding: 2rem 1.25rem 1.25rem;
  }
}

</style>