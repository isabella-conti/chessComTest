<template>
  <div class="sidebar" ref="sidebar">
    <h3>Selected Squares:</h3>
    <ul>
      <li v-for="(square, index) in squares" :key="index">
        {{ index + 1 }}. {{ square }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue'

const props = defineProps({
  squares: {
    type: Array,
    required: true
  }
})

const sidebar = ref(null)

watch(
  () => props.squares.length,
  async () => {
    await nextTick()
    if (sidebar.value) {
      sidebar.value.scrollTop = sidebar.value.scrollHeight
    }
  }
)
</script>

<style scoped>
.sidebar {
  color: white;
  background-color: #2c2c2c;
  padding: 0.9375rem;
  border-radius: 0.5rem;
  width: 10rem;
  height: 32rem;
  overflow-y: auto;
  overflow-x: hidden;
  box-sizing: border-box;
}

.sidebar ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.sidebar li {
  padding: 0.375rem 0;
  border-bottom: 0.0625rem solid rgba(255, 255, 255, 0.06);
}

.sidebar::-webkit-scrollbar {
  width: 0.5rem;
}

.sidebar::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.12);
  border-radius: 0.25rem;
}
</style>
