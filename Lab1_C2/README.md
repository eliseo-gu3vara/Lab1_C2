# Reservas de salas de estudio (Lab1_C2)

En muchas bibliotecas universitarias y centros de estudio, los estudiantes deben reservar salas para trabajar en grupo. Cuando el proceso se hace en hojas de papel o por mensajería, se generan conflictos de horarios, duplicados y dificultades para saber quién está esperando.

Esta aplicación propone una solución web sencilla: permitir ingresar una reserva indicando el nombre del estudiante, la sala y la hora de inicio. El sistema muestra las reservas registradas, permite marcar la llegada del estudiante y cancelar reservas si es necesario.

### Sectores beneficiados

- **Educación**: bibliotecas y centros de estudio que necesitan gestionar el uso de espacios de trabajo.
- **Gestión de instalaciones**: cualquier organización que requiera reservar salas o equipos y desee evitar solapamientos.

### Funciones que implementa la aplicación

- Registrar una reserva con nombre, sala y hora de inicio.
- Validar que los datos obligatorios estén completos antes de guardar.
- Evitar que se creen reservas duplicadas para la misma sala en la misma hora.
- Mostrar la lista de reservas y permitir filtrarla por estado (por iniciar / registrados).
- Marcar que un estudiante ya se presentó (check-in) y cancelar reservas existentes.

---

## ¿Qué es Vue.js y qué hace en esta página?

Vue.js es un framework de JavaScript que facilita construir interfaces interactivas y reactivas. En esta página, Vue mantiene sincronizados los datos y la vista: al escribir el nombre o elegir una sala, se actualizan las variables reactivas; al registrar una reserva, la lista se actualiza automáticamente; y al marcar la llegada de una persona, la tarjeta cambia de estado sin recargar la página.

## Variables reactivas utilizadas y su función dentro del sistema

- `studentName`: almacena el nombre del estudiante que hace la reserva.
- `studentCode`: guarda el código del estudiante para identificar la reserva.
- `room`: indica la sala elegida (ej. A1, B2, C3).
- `startTime`: momento en el que comienza la reserva.
- `reservations`: arreglo con todas las reservas registradas.
- `view`: controla el filtro que se aplica a la vista (todas / por iniciar / registradas).
- `errorMessage`: muestra un mensaje cuando la validación detecta datos incompletos.

## Diferencia entre `v-bind` y `v-model` en el proyecto

`v-model` se usa para enlazar los campos del formulario con las variables reactivas (`studentName`, `studentCode`, `room`, `startTime`). Esto permite que cada vez que el usuario escribe o cambia una selección, la variable se actualice inmediatamente.

`v-bind` se usa para enlazar atributos de elementos a valores dinámicos. Por ejemplo, se usa `:disabled="isSubmitDisabled"` en el botón de guardar para desactivarlo cuando faltan datos, y `:class="{ completed: reservation.checkedIn }"` para cambiar el estilo según si la persona ya se registró.

## Ejemplo de evento utilizado en la aplicación

La aplicación usa `@submit.prevent` en el formulario para ejecutar `addReservation()` sin recargar la página. También utiliza `@click` en botones para registrar la llegada (`toggleCheckIn`) y cancelar una reserva (`cancelReservation`).

## Uso de `v-for` en la aplicación

`v-for` renderiza una tarjeta por cada reserva en la lista (`filteredReservations`). Al iterar sobre el arreglo, la interfaz muestra dinámicamente todas las reservas actuales sin escribir manualmente cada sección.

## Uso de `v-if` y el problema que resuelve

`v-if` se usa para mostrar un mensaje cuando no hay reservas registradas (`v-if="filteredReservations.length === 0"`). También se usa para mostrar el mensaje de error (`v-if="errorMessage"`). Así se evita mostrar contenido vacío o irrelevante cuando no hay datos.

## Validación de datos

Antes de registrar una reserva, se comprueba que el nombre del estudiante no esté vacío, que se haya elegido una sala y que se haya indicado una hora. Además, se verifica que no exista otra reserva con la misma sala y hora para evitar solapamientos.

Si falta algún dato o hay un conflicto de horario, se muestra un mensaje de error y no se guarda la reserva.

La validación es importante porque evita que se creen reservas incompletas o duplicadas, lo que puede generar confusiones y conflictos de horarios en la gestión de salas.

---

## Cómo ejecutar el proyecto

```sh
npm install
npm run dev
```
