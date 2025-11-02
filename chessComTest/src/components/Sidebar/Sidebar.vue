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
  width: clamp(6rem, calc(var(--board-size-rem, 16rem) * 0.28), 14rem);
  height: calc(var(--board-size-rem, 16rem));
}

.sidebar--bottom {
  width: 100%;
  height: clamp(3rem, calc(var(--board-size-rem, 16rem) * 0.12), 8rem);
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

@media (max-width: 40rem) {
  .sidebar--right {
    width: clamp(5.5rem, calc(var(--board-size-rem, 12rem) * 0.28), 10rem);
    height: calc(var(--board-size-rem, 12rem));
    padding: 0.5rem;
    font-size: 0.875rem;
  }

  .sidebar--bottom {
    height: clamp(2.5rem, calc(var(--board-size-rem, 12rem) * 0.12), 6rem);
  }

  .sidebar h3 {
    font-size: 0.95rem;
    margin-bottom: 0.25rem;
  }

  .sidebar li {
    padding: 0.25rem 0;
    border-bottom-width: 0.04rem;
  }

  .sidebar::-webkit-scrollbar {
    width: 0.35rem;
  }
}

@media (max-width: 26.25rem) {
  .sidebar--right {
    width: clamp(5rem, calc(var(--board-size-rem, 10rem) * 0.28), 8rem);
    height: calc(var(--board-size-rem, 10rem));
    font-size: 0.8rem;
  }

  .sidebar--bottom {
    height: clamp(2rem, calc(var(--board-size-rem, 10rem) * 0.12), 5.5rem);
  }

  .sidebar h3 {
    font-size: 0.9rem;
  }
}
</style>
