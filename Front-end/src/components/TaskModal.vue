<script setup>
import { ref } from 'vue'
import { format } from 'date-fns'

const props = defineProps({
  date: {
    type: Date,
    required: true
  }
})

const emit = defineEmits(['close', 'submit'])

const formData = ref({
  patientName: '',
  appointmentType: 'consultation',
  date: format(props.date, 'yyyy-MM-dd'),
  location: '',
  notes: '',
  phoneNumber: ''
})

const handleSubmit = () => {
  emit('submit', { ...formData.value })
  formData.value = {
    patientName: '',
    appointmentType: 'consultation',
    date: format(props.date, 'yyyy-MM-dd'),
    location: '',
    notes: '',
    phoneNumber: ''
  }
}
</script>

<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content">
      <div class="modal-header">
        <h2>New Appointment</h2>
        <button class="close-button" @click="$emit('close')">&times;</button>
      </div>

      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="patientName">Patient Name</label>
          <input
            type="text"
            id="patientName"
            v-model="formData.patientName"
            placeholder="Enter patient name"
          >
        </div>

        <div class="form-group">
          <label for="appointmentType">Appointment Type</label>
          <select id="appointmentType" v-model="formData.appointmentType">
            <option value="consultation">Consultation</option>
            <option value="surgery">Surgery</option>
          </select>
        </div>

        <div class="form-group">
          <label for="date">Date</label>
          <input
            type="date"
            id="date"
            v-model="formData.date"
            readonly
          >
        </div>

        <div class="form-group">
          <label for="location">Location</label>
          <input
            type="text"
            id="location"
            v-model="formData.location"
            placeholder="Enter location"
          >
        </div>

        <div class="form-group">
          <label for="phoneNumber">Phone Number</label>
          <input
            type="tel"
            id="phoneNumber"
            v-model="formData.phoneNumber"
            placeholder="Enter phone number"
          >
        </div>

        <div class="form-group">
          <label for="notes">Notes</label>
          <textarea
            id="notes"
            v-model="formData.notes"
            placeholder="Enter any additional notes"
            rows="3"
          ></textarea>
        </div>

        <div class="form-actions">
          <button type="button" class="cancel-btn" @click="$emit('close')">Cancel</button>
          <button type="submit" class="submit-btn">Save Appointment</button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.75);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: #2a2a2a;
  border-radius: 8px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-bottom: 1px solid #333;
}

.modal-header h2 {
  margin: 0;
  color: white;
}

.close-button {
  background: none;
  border: none;
  color: #888;
  font-size: 1.5em;
  cursor: pointer;
}

form {
  padding: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  color: #888;
}

input,
select,
textarea {
  width: 100%;
  padding: 8px 12px;
  background-color: #333;
  border: 1px solid #444;
  border-radius: 4px;
  color: white;
}

input:focus,
select:focus,
textarea:focus {
  outline: none;
  border-color: #666;
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
}

button {
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  border: none;
  font-weight: 500;
}

.cancel-btn {
  background-color: #444;
  color: white;
}

.submit-btn {
  background-color: #4a4a4a;
  color: white;
}

.submit-btn:hover {
  background-color: #555;
}
</style>