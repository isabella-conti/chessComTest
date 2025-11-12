<template>
  <div :class="['sidebar', `sidebar--${props.position}`]" ref="sidebar">
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
  },
  position: {
    type: String,
    default: 'right'
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
  color: #ffffff;
  background-color: #2c2c2c;
  padding: 0.9375rem;
  border-radius: 0.5rem;
  overflow-y: auto;
  overflow-x: hidden;
  box-sizing: border-box;
}

.sidebar--right {
  width: 14rem;
  height: min(85vw, 85vh - 4rem, 40rem);
  margin: 0;
}

.sidebar--bottom {
  width: 100%;
  max-width: min(90vw, 28rem);
  height: 8rem;
  margin: 0 auto;
}

.sidebar ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.sidebar li {
  padding: 0.375rem 0;
  border-bottom: 0.0625rem solid rgba(255, 255, 255, 0.06);
  font-size: 0.875rem;
}

.sidebar::-webkit-scrollbar {
  width: 0.5rem;
}

.sidebar::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.12);
  border-radius: 0.25rem;
}

@media (max-width: 56rem) {
  .sidebar--right {
    width: 100%;
    max-width: min(90vw, 28rem);
    height: 8rem;
    margin: 0 auto;
  }
}

</style>