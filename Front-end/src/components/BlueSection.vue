<script setup>
import { ref, computed } from 'vue'
import { format } from 'date-fns'
import PatientModal from './PatientModal.vue'

const props = defineProps({
  appointments: {
    type: Array,
    default: () => []
  }
})

const selectedPatient = ref(null)
const isEditing = ref(false)
const showModal = ref(false)

const sortedUsers = computed(() => {
  return [...props.appointments].sort((a, b) => {
    return new Date(a.date) - new Date(b.date)
  }).map(appointment => ({
    name: appointment.patientName,
    date: format(new Date(appointment.date), 'dd/MM/yyyy'),
    type: appointment.appointmentType,
    location: appointment.location,
    notes: appointment.notes,
    phoneNumber: appointment.phoneNumber
  }))
})

const openPatientModal = (patient) => {
  selectedPatient.value = patient
  isEditing.value = false
  showModal.value = true
}

const handleEdit = () => {
  isEditing.value = true
}

const handleUpdate = (updatedPatient) => {
  // Here you would typically update the patient data in your state management
  console.log('Updated patient:', updatedPatient)
  showModal.value = false
  isEditing.value = false
}

const closeModal = () => {
  showModal.value = false
  isEditing.value = false
  selectedPatient.value = null
}
</script>

<template>
  <section class="blue-section">
    <div class="users-list">
      <h2>Scheduled Patients</h2>
      <div class="user-cards">
        <div 
          v-for="user in sortedUsers" 
          :key="user.name + user.date" 
          class="user-card"
          @click="openPatientModal(user)"
        >
          <div class="user-info">
            <h3>{{ user.name }}</h3>
            <p class="date">{{ user.date }}</p>
            <p class="type">{{ user.type }}</p>
            <p class="location" v-if="user.location">{{ user.location }}</p>
          </div>
        </div>
      </div>
    </div>

    <PatientModal
      v-if="showModal && selectedPatient"
      :patient="selectedPatient"
      :isEditing="isEditing"
      @close="closeModal"
      @edit="handleEdit"
      @update="handleUpdate"
    />
  </section>
</template>

<style scoped>
.blue-section {
  width: calc(640 * 100% / 1920);
  height: 100%;
  background-color: #1a1a1a;
  padding: 20px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.users-list {
  color: white;
  flex: 1;
  overflow-y: auto;
}

.users-list h2 {
  margin-bottom: 20px;
  font-size: 1.5em;
  color: #fff;
  position: sticky;
  top: 0;
  background-color: #1a1a1a;
  padding: 10px 0;
  z-index: 1;
}

.user-cards {
  display: flex;
  flex-direction: column;
  gap: 15px;
  padding-bottom: 20px;
}

.user-card {
  background-color: #2a2a2a;
  border-radius: 8px;
  padding: 15px;
  transition: transform 0.2s ease;
  cursor: pointer;
}

.user-card:hover {
  transform: translateX(5px);
  background-color: #333;
}

.user-info h3 {
  margin: 0;
  color: #fff;
  font-size: 1.2em;
}

.user-info p {
  margin: 5px 0;
  color: #888;
  font-size: 0.9em;
}

.date {
  color: #666;
}

.type {
  text-transform: capitalize;
}

.location {
  font-style: italic;
}

@media (max-width: 768px) {
  .blue-section {
    width: 100%;
    height: calc(980px * 33.33vh / 1080);
  }
}
</style>