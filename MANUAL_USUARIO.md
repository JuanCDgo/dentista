# 🦷 Manual de Usuario · Consultorio Odontológico

¡Bienvenido a la aplicación web de gestión de pacientes, recordatorios de control y turnero en la nube para tu consultorio odontológico! Diseñada para brindar rapidez en el consultorio, facilidad para agendar pacientes y cobro ágil sin complicaciones técnicas.

---

## 1. Pantalla Principal: Módulos y Navegación

1. **Encabezado Institucional:**
   * **Nombre y Logo:** Personalizables desde los ajustes con el nombre del profesional/clínica y foto/logo del consultorio.
   * **Badge de Turnero:** Muestra en tiempo real si el turnero online para pacientes está activo (`🟢 Turnero Activo`) o pausado.
   * **📷 Cobro QR:** Acceso directo para mostrar en pantalla el código QR de cobro para transferencias o Mercado Pago.
   * **⚙️ Ajustes:** Configuración del consultorio, mensajes de WhatsApp, horarios de atención y copia en la nube.

2. **Panel de Pacientes:**
   * Lista completa de pacientes registrados con fecha de última visita, tratamiento realizado y semanas transcurridas.
   * **Diseño limpio:** Solo resalta en rojo si el paciente registra saldo o cuota pendiente por un tratamiento (con acceso directo al cobro QR).
   * **Filtros rápidos:** Filtrar por pacientes para control semestral, tratamientos en curso o con saldo pendiente.

---

## 2. Flujo de Atención y Recordatorios

### Registrar o Actualizar Paciente
1. Tocá el botón verde **+ Nuevo Paciente**.
2. Cargá el **Nombre completo** y su número de **WhatsApp** con código de área (ej: `5491122334455`).
3. Indicá el tratamiento realizado (ej: *Limpieza Dental*, *Control Semestral*, *Blanqueamiento*, *Arreglo de Caries*).
4. Guardá la ficha.

### Enviar Recordatorio por WhatsApp en 1 Toque
1. En la ficha del paciente vas a ver el indicador de tiempo (ej: `Hace 24 semanas` para controles periódicos).
2. Tocá el botón verde de **WhatsApp**.
3. Se abrirá la conversación con un mensaje personalizado y profesional:
   > *"¡Hola Laura! Ya pasaron 24 semanas de tu Limpieza Dental, ¿te reservo turno para tu control odontológico?"*
4. Enviás el mensaje con 1 toque sin tener que redactar nada.

---

## 3. Turnero Online para Pacientes (Nube con Bloqueo Real)

* **Enlace para Pacientes:** Podés compartir el enlace público por WhatsApp o Instagram para que los pacientes elijan su horario de consulta.
* **Bloqueo Real Instantáneo:** Apenas un paciente reserva un horario (ej: martes 15:00 hs), se bloquea automáticamente en la nube y aparece deshabilitado (`🔒 Ocupado`) para cualquier otra persona.
* **Freno de Colisión:** Si dos pacientes entran a la vez sobre el mismo casillero, el sistema evita que se superpongan y notifica al instante.
* **📅 Panel de Agenda del Odontólogo:**
   * Desde **📅 Control Manual de Horarios** podés bloquear horarios ocupados telefónicamente, liberar turnos cancelados o cerrar días enteros por congresos, cirugías o feriados.

---

## 4. Cobro con QR en Consultorio

* Al terminar la consulta o sesión, tocá **📷 Cobro QR** o el botón de cobro en la ficha del paciente.
* El sistema muestra en grande el código QR vinculado a tu Alias/CBU bancario con el monto exacto en pesos argentinos (ARS $).
* El paciente escanea desde su app bancaria o billetera virtual y el pago ingresa directo a tu cuenta.
* Cuenta con fallback redundante automático: si una vía no responde, se conmuta al instante a un generador alternativo.

---

## 5. Respaldo y Seguridad de Historias / Fichas

* **Copia en Google Drive:** Sincronización automática periódica mediante webhook para resguardo seguro fuera del dispositivo.
* **Exportar / Importar JSON:** Opción de descarga directa de un archivo de respaldo con un solo clic.
