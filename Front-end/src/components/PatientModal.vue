<script setup>
import { ref } from 'vue'

const props = defineProps({
  patient: {
    type: Object,
    required: true
  },
  isEditing: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close', 'update'])

const formData = ref({
  patientName: props.patient.name,
  appointmentType: props.patient.type,
  date: props.patient.date,
  location: props.patient.location || '',
  notes: props.patient.notes || '',
  phoneNumber: props.patient.phoneNumber || ''
})

const toggleEdit = () => {
  emit('update', { ...formData.value })
}
</script>

<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content">
      <div class="modal-header">
        <h2>Patient Details</h2>
        <button class="close-button" @click="$emit('close')">&times;</button>
      </div>

      <div class="patient-details">
        <div class="form-group">
          <label>Patient Name</label>
          <input
            type="text"
            v-model="formData.patientName"
            :readonly="!isEditing"
            :class="{ 'readonly': !isEditing }"
          >
        </div>

        <div class="form-group">
          <label>Appointment Type</label>
          <input
            type="text"
            v-model="formData.appointmentType"
            readonly
            class="readonly"
          >
        </div>

        <div class="form-group">
          <label>Date</label>
          <input
            type="text"
            v-model="formData.date"
            readonly
            class="readonly"
          >
        </div>

        <div class="form-group">
          <label>Location</label>
          <input
            type="text"
            v-model="formData.location"
            :readonly="!isEditing"
            :class="{ 'readonly': !isEditing }"
          >
        </div>

        <div class="form-group">
          <label>Phone Number</label>
          <input
            type="tel"
            v-model="formData.phoneNumber"
            :readonly="!isEditing"
            :class="{ 'readonly': !isEditing }"
          >
        </div>

        <div class="form-group">
          <label>Notes</label>
          <textarea
            v-model="formData.notes"
            :readonly="!isEditing"
            :class="{ 'readonly': !isEditing }"
            rows="3"
          ></textarea>
        </div>

        <div class="form-actions">
          <button 
            v-if="!isEditing" 
            type="button" 
            class="edit-btn"
            @click="$emit('edit')"
          >
            Edit
          </button>
          <template v-else>
            <button type="button" class="cancel-btn" @click="$emit('close')">Cancel</button>
            <button type="button" class="submit-btn" @click="toggleEdit">Save Changes</button>
          </template>
        </div>
      </div>
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

.patient-details {
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
textarea {
  width: 100%;
  padding: 8px 12px;
  background-color: #333;
  border: 1px solid #444;
  border-radius: 4px;
  color: white;
}

input.readonly,
textarea.readonly {
  background-color: #2a2a2a;
  border-color: #333;
  cursor: default;
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

.edit-btn {
  background-color: #4a4a4a;
  color: white;
}

.cancel-btn {
  background-color: #444;
  color: white;
}

.submit-btn {
  background-color: #4a4a4a;
  color: white;
}

.submit-btn:hover,
.edit-btn:hover {
  background-color: #555;
}
</style>