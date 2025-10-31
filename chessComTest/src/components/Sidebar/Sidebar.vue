<template>
  <div class="sidebar">
    <h3>Selected Squares:</h3>
    <ul>
      <li v-for="(square, index) in squares" :key="index">
        {{ index + 1 }}. {{ square }}
      </li>
    </ul>
  </div>
</template>

<script setup>
defineProps({
  squares: {
    type: Array,
    required: true
  }
})

</script>

<style scoped>
.sidebar {
  color: white;
  background-color: #2c2c2c;
  padding: 15px;
  border-radius: 8px;
  min-width: 150px;
  max-width: 200px;
  flex: 1;
}

</style>
