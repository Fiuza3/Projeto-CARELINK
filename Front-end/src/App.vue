<script setup>
import { ref, provide } from 'vue'
import RedHeader from './components/RedHeader.vue'
import BlueSection from './components/BlueSection.vue'
import GreenSection from './components/GreenSection.vue'

const appointments = ref([])

const updateAppointments = (newAppointment) => {
  appointments.value.push(newAppointment)
}

provide('updateAppointments', updateAppointments)
</script>

<template>
  <div class="layout">
    <RedHeader />
    <div class="content-wrapper">
      <BlueSection :appointments="appointments" />
      <GreenSection />
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #242424;
}

.layout {
  width: 100vw;
  height: 100vh;
  max-width: 1920px;
  max-height: 1080px;
  display: flex;
  flex-direction: column;
  aspect-ratio: 16/9;
}

.content-wrapper {
  display: flex;
  flex-direction: row;
  flex: 1;
}

@media (max-width: 768px) {
  .content-wrapper {
    flex-direction: column;
  }
}
</style>