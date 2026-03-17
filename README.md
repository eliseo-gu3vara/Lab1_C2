**Reservas de cubículos de la Biblioteca UGB**

En la Biblioteca de la Universidad Gerardo Barrios (UGB), los estudiantes necesitan reservar cubículos para trabajar de forma individual o en grupo. Cuando este proceso se gestiona en papel o por mensajería, se generan conflictos de horarios, reservas duplicadas y dificultades para saber qué cubículos están disponibles o quién está en lista de espera.
Esta aplicación propone una solución web sencilla para la Biblioteca UGB: permitir registrar una reserva indicando el nombre del estudiante, el cubículo deseado y la hora de inicio. El sistema muestra las reservas activas, permite confirmar la llegada del estudiante y cancelar reservas cuando sea necesario.



**Sectores beneficiados**

Educación: la Biblioteca UGB y sus estudiantes, quienes podrán gestionar el uso de los cubículos de estudio de forma ordenada y sin conflictos.
Gestión de instalaciones: el personal bibliotecario de la UGB que necesita supervisar la disponibilidad de los espacios y evitar solapamientos en las reservas.



**Funciones que implementa la aplicación**

Registrar una reserva con el nombre del estudiante, el cubículo de la Biblioteca UGB y la hora de inicio.
Validar que todos los datos obligatorios estén completos antes de guardar la reserva.
Evitar que se creen reservas duplicadas para el mismo cubículo en el mismo horario.
Mostrar la lista de reservas y permitir filtrarla por estado (por iniciar / registrados).
Marcar que el estudiante ya se presentó al cubículo (check-in) y cancelar reservas si es necesario.



**¿Qué es Vue.js y qué hace en esta página?**

Vue.js es un framework de JavaScript que facilita construir interfaces interactivas y reactivas. En esta página, Vue mantiene sincronizados los datos y la vista: al escribir el nombre del estudiante o elegir un cubículo de la Biblioteca UGB, se actualizan las variables reactivas; al registrar una reserva, la lista se actualiza automáticamente; y al confirmar la llegada del estudiante, la tarjeta cambia de estado sin recargar la página.



**Variables reactivas utilizadas y su función dentro del sistema**

studentName: almacena el nombre del estudiante de la UGB que realiza la reserva.
studentCode: guarda el carné universitario del estudiante para identificar la reserva.
room: indica el cubículo elegido dentro de la Biblioteca UGB (ej. C1, C2, C3).
startTime: momento en el que inicia la reserva del cubículo.
reservations: arreglo con todas las reservas registradas en el sistema.
view: controla el filtro aplicado a la vista (todas / por iniciar / registradas).
errorMessage: muestra un mensaje cuando la validación detecta datos incompletos o un conflicto de horario.



**Diferencia entre v-bind y v-model en el proyecto**

v-model se usa para enlazar los campos del formulario con las variables reactivas (studentName, studentCode, room, startTime). Esto permite que cada vez que el estudiante de la UGB escribe su nombre, su carné o selecciona un cubículo, la variable se actualice de inmediato. v-bind se usa para enlazar atributos de elementos a valores dinámicos. Por ejemplo, :disabled="isSubmitDisabled" desactiva el botón de guardar cuando faltan datos, y :class="{ completed: reservation.checkedIn }" cambia el estilo visual de la tarjeta cuando el estudiante ya se presentó al cubículo.



**Ejemplo de evento utilizado en la aplicación**

La aplicación usa @submit.prevent en el formulario para ejecutar addReservation() sin recargar la página. También utiliza @click en los botones para registrar la llegada del estudiante al cubículo (toggleCheckIn) y para cancelar una reserva existente (cancelReservation).



**Uso de v-for en la aplicación**

v-for renderiza una tarjeta por cada reserva activa de los cubículos de la Biblioteca UGB (filteredReservations). Al iterar sobre el arreglo, la interfaz muestra dinámicamente todas las reservas actuales sin necesidad de escribir manualmente cada sección



**Uso de v-if y el problema que resuelve**

v-if se usa para mostrar un mensaje cuando no hay reservas registradas en la Biblioteca UGB (v-if="filteredReservations.length === 0"). También se usa para mostrar el mensaje de error (v-if="errorMessage"). Así se evita mostrar contenido vacío o irrelevante cuando no existen reservas activas.


**Validación de datos**

Antes de registrar una reserva de cubículo, se verifica que el nombre del estudiante no esté vacío, que se haya seleccionado un cubículo disponible en la Biblioteca UGB y que se haya indicado una hora de inicio. Además, se comprueba que no exista otra reserva para el mismo cubículo en el mismo horario, evitando solapamientos.
Si falta algún dato o se detecta un conflicto, se muestra un mensaje de error y la reserva no se guarda.
La validación es fundamental para garantizar una gestión ordenada de los cubículos de la Biblioteca UGB, evitando confusiones y asegurando que cada estudiante tenga un espacio disponible en el horario reservado.
