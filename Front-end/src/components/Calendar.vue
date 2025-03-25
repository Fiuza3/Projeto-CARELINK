<script setup>
import { ref, computed, inject } from 'vue'
import { 
  startOfMonth,
  endOfMonth,
  startOfWeek,
  endOfWeek,
  eachDayOfInterval,
  format,
  addMonths,
  subMonths,
  isSameMonth,
  isToday
} from 'date-fns'
import TaskModal from './TaskModal.vue'

const currentDate = ref(new Date())
const selectedDate = ref(null)
const showModal = ref(false)
const appointments = ref([])

const updateAppointments = inject('updateAppointments')

const weeks = computed(() => {
  const start = startOfWeek(startOfMonth(currentDate.value))
  const end = endOfWeek(endOfMonth(currentDate.value))
  const days = eachDayOfInterval({ start, end })

  const weeks = []
  let currentWeek = []

  days.forEach(day => {
    if (currentWeek.length === 7) {
      weeks.push(currentWeek)
      currentWeek = []
    }
    currentWeek.push(day)
  })

  if (currentWeek.length > 0) {
    weeks.push(currentWeek)
  }

  return weeks
})

const nextMonth = () => {
  currentDate.value = addMonths(currentDate.value, 1)
}

const previousMonth = () => {
  currentDate.value = subMonths(currentDate.value, 1)
}

const selectDate = (date) => {
  selectedDate.value = date
  showModal.value = true
}

const handleTaskSubmit = (taskData) => {
  appointments.value.push({
    ...taskData,
    date: selectedDate.value
  })
  updateAppointments({
    ...taskData,
    date: selectedDate.value
  })
  showModal.value = false
}

const getAppointmentsForDay = (date) => {
  return appointments.value.filter(app => 
    format(new Date(app.date), 'yyyy-MM-dd') === format(date, 'yyyy-MM-dd')
  )
}
</script>

<template>
  <div class="calendar">
    <div class="calendar-header">
      <button @click="previousMonth">&lt;</button>
      <h2>{{ format(currentDate, 'MMMM yyyy') }}</h2>
      <button @click="nextMonth">&gt;</button>
    </div>

    <div class="calendar-grid">
      <div class="weekday" v-for="day in ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']">
        {{ day }}
      </div>

      <template v-for="week in weeks">
        <div
          v-for="day in week"
          :key="day"
          class="day"
          :class="{
            'current-month': isSameMonth(day, currentDate),
            'today': isToday(day)
          }"
          @click="selectDate(day)"
        >
          <span class="day-number">{{ format(day, 'd') }}</span>
          <div class="appointments">
            <div v-for="appointment in getAppointmentsForDay(day)" 
                 :key="appointment.patientName"
                 class="appointment">
              {{ appointment.patientName }}
            </div>
          </div>
        </div>
      </template>
    </div>

    <TaskModal
      v-if="showModal"
      :date="selectedDate"
      @close="showModal = false"
      @submit="handleTaskSubmit"
    />
  </div>
</template>

<style scoped>
.calendar {
  height: 100%;
  display: flex;
  flex-direction: column;
  background-color: #2a2a2a;
  border-radius: 8px;
  overflow: hidden;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background-color: #333;
}

.calendar-header button {
  background: none;
  border: none;
  color: white;
  font-size: 1.5em;
  cursor: pointer;
  padding: 5px 15px;
}

.calendar-header h2 {
  color: white;
  margin: 0;
}

.calendar-grid {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 1px;
  background-color: #333;
  padding: 1px;
}

.weekday {
  background-color: #3a3a3a;
  padding: 10px;
  text-align: center;
  color: #888;
  font-weight: 500;
}

.day {
  background-color: #2a2a2a;
  min-height: 100px;
  padding: 8px;
  cursor: pointer;
  position: relative;
  display: flex;
  flex-direction: column;
}

.day:hover {
  background-color: #333;
}

.day-number {
  color: #888;
  font-size: 0.9em;
}

.current-month .day-number {
  color: white;
}

.today {
  background-color: #333;
}

.appointments {
  margin-top: 4px;
  overflow: hidden;
}

.appointment {
  background-color: #444;
  color: white;
  padding: 2px 4px;
  border-radius: 3px;
  margin: 2px 0;
  font-size: 0.8em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>