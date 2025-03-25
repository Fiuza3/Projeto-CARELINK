<script setup>
import Calendar from './Calendar.vue'
</script>

<template>
  <section class="green-section">
    <Calendar />
  </section>
</template>

<style scoped>
.green-section {
  width: calc(1280 * 100% / 1920);
  height: 100%;
  background-color: #1a1a1a;
  padding: 20px;
}

@media (max-width: 768px) {
  .green-section {
    width: 100%;
    height: calc(980px * 66.67vh / 1080);
  }
}
</style>