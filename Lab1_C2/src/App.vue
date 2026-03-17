<script setup>
import { ref, computed } from 'vue';

const studentName = ref('');
const studentCode = ref('');
const room = ref('');
const startTime = ref('');
const reservations = ref([]);
const view = ref('all');
const errorMessage = ref('');

const filteredReservations = computed(() => {
  if (view.value === 'active') return reservations.value.filter((r) => !r.checkedIn);
  if (view.value === 'checked-in') return reservations.value.filter((r) => r.checkedIn);
  return reservations.value;
});

const isSubmitDisabled = computed(() => {
  return (
    !studentName.value.trim() ||
    !studentCode.value.trim() ||
    !room.value ||
    !startTime.value
  );
});

function resetForm() {
  studentName.value = '';
  studentCode.value = '';
  room.value = '';
  startTime.value = '';
  errorMessage.value = '';
}

function validate() {
  if (!studentName.value.trim()) {
    errorMessage.value = 'El nombre del estudiante es obligatorio.';
    return false;
  }

  if (!studentCode.value.trim()) {
    errorMessage.value = 'El código de estudiante es obligatorio.';
    return false;
  }

  if (!room.value) {
    errorMessage.value = 'Selecciona un cubiculo.';
    return false;
  }

  if (!startTime.value) {
    errorMessage.value = 'Indica la hora de inicio.';
    return false;
  }

  // Evita reservas duplicadas en la misma sala y hora
  const conflict = reservations.value.some(
    (r) => r.room === room.value && r.startTime === startTime.value
  );
  if (conflict) {
    errorMessage.value = 'Ya existe una reserva para esa sala en esa hora.';
    return false;
  }

  return true;
}

function addReservation() {
  errorMessage.value = '';
  if (!validate()) return;

  reservations.value.push({
    id: Date.now(),
    studentName: studentName.value.trim(),
    studentCode: studentCode.value.trim(),
    room: room.value,
    startTime: startTime.value,
    checkedIn: false,
  });

  resetForm();
}

function toggleCheckIn(reservation) {
  reservation.checkedIn = !reservation.checkedIn;
}

function cancelReservation(reservationId) {
  reservations.value = reservations.value.filter((r) => r.id !== reservationId);
}
</script>

<template>
  <section class="app">
    <header>
      <h1>Reservas de cubiculos</h1>
      <p>
        Este sistema permite llevar el control de las reservas de cubiculos de trabajo en la biblioteca UGB,
        registrando quién la ocupará y a qué hora debe iniciar.
      </p>
    </header>

    <form class="order-form" @submit.prevent="addReservation">
      <div class="form-row">
        <label for="student">Nombre del estudiante</label>
        <input
          id="student"
          type="text"
          v-model="studentName"
          placeholder="Ej. Ana Pérez"
          autocomplete="off"
        />
      </div>

      <div class="form-row">
        <label for="code">Código del estudiante</label>
        <input
          id="code"
          type="text"
          v-model="studentCode"
          placeholder="Ej. SMSS012625"
          autocomplete="off"
        />
      </div>

      <div class="form-row">
        <label for="room">Sala</label>
        <select id="room" v-model="room">
          <option value="" disabled>Seleccione una sala</option>
          <option value="A1">Sala A1</option>
          <option value="B2">Sala B2</option>
          <option value="C3">Sala C3</option>
        </select>
      </div>

      <div class="form-row">
        <label for="start">Hora de inicio</label>
        <input id="start" type="time" v-model="startTime" />
      </div>

      <div class="form-row actions">
        <button type="submit" :disabled="isSubmitDisabled">Registrar reserva</button>
        <button type="button" @click="resetForm">Limpiar</button>
      </div>

      <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
    </form>

    <section class="orders">
      <div class="orders-header">
        <h2>Reservas</h2>
        <div class="filters">
          <label for="view">Ver:</label>
          <select id="view" v-model="view">
            <option value="all">Todas</option>
            <option value="active">Por iniciar</option>
            <option value="checked-in">Registradas</option>
          </select>
        </div>
      </div>

      <p class="empty" v-if="filteredReservations.length === 0">
        Aún no hay reservas. Agrega una para comenzar.
      </p>

      <ul class="order-list">
        <li
          v-for="reservation in filteredReservations"
          :key="reservation.id"
          :class="{ completed: reservation.checkedIn }"
        >
          <header class="order-header">
            <h3>{{ reservation.studentName }}</h3>
            <span
              class="badge"
              v-bind:title="reservation.checkedIn ? 'Estudiante registrado' : 'Pendiente de llegada'"
            >
              {{ reservation.checkedIn ? 'Registrado' : 'Por iniciar' }}
            </span>
          </header>

          <p class="details">
            Código: <strong>{{ reservation.studentCode }}</strong> · Sala: <strong>{{ reservation.room }}</strong> · Hora: <strong>{{ reservation.startTime }}</strong>
          </p>

          <div class="order-actions">
            <button type="button" @click="toggleCheckIn(reservation)">
              {{ reservation.checkedIn ? 'Marcar como no registrado' : 'Registrar llegada' }}
            </button>
            <button type="button" @click="cancelReservation(reservation.id)">Cancelar</button>
          </div>
        </li>
      </ul>
    </section>

    <footer>
      <p>
      Todos los derechos reservados © 2026. Desarrollado por Roberto Lopez y Eliseo Guevara.
      </p>
    </footer>
  </section>
</template>

<style scoped>
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 1.5rem;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

header h1 {
  margin-bottom: 0.25rem;
}

header p {
  margin-top: 0;
  color: #555;
}

.order-form {
  margin: 1.5rem 0;
  border: 1px solid #ddd;
  border-radius: 0.5rem;
  padding: 1rem;
  background: #fafafa;
}

.form-row {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  margin-bottom: 0.75rem;
}

.form-row label {
  font-weight: 600;
}

.form-row input,
.form-row textarea,
.form-row select {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.35rem;
}

.actions {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

button {
  padding: 0.55rem 0.75rem;
  border: none;
  border-radius: 0.35rem;
  background: #3b82f6;
  color: white;
  cursor: pointer;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.55;
}

button[type='button'] {
  background: #6b7280;
}

.error {
  margin: 0.5rem 0 0;
  color: #b91c1c;
}

.orders-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.filters {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.order-list {
  list-style: none;
  padding: 0;
  margin: 1rem 0 0;
  display: grid;
  gap: 0.75rem;
}

.order-list li {
  border: 1px solid #ddd;
  border-radius: 0.5rem;
  padding: 0.75rem;
  background: white;
}

.order-list li.completed {
  opacity: 0.7;
  background: #f0fdf4;
}

.order-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 1rem;
}

.badge {
  font-size: 0.75rem;
  padding: 0.25rem 0.5rem;
  border-radius: 999px;
  background: #e5e7eb;
}

.order-actions {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.75rem;
}

.order-actions button {
  flex: 1;
  background: #10b981;
}

.order-actions button:last-child {
  background: #ef4444;
}

.empty {
  color: #6b7280;
  font-style: italic;
}
</style>
