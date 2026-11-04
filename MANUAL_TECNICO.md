# 🛠️ Manual Técnico · Consultorio Odontológico

**Stack:** HTML5, Tailwind CSS (CDN), Vanilla JavaScript (ES6+), LocalStorage, Cloudflare Worker / JSONBin Cloud API, QRCode.js, Canvas Confetti.  
**Despliegue:** GitHub Pages (`https://juancdgo.github.io/dentista/`).  
**Copia de Seguridad:** Google Drive / LocalStorage JSON.

---

## 1. Arquitectura de la Solución

Aplicación **Single-Page Application (SPA)** de arquitectura desacoplada y ejecución 100% en el cliente del navegador:
* Independiente de servidores centrales pesados.
* Motor de persistencia primaria en `localStorage` con redundancia en la nube.
* Capa de sincronización de turnos en tiempo real mediante Worker sin estado.

### Estructura de Directorios:
```text
Mini Apps/dentista/
├── index.html            # Aplicación completa (Fichas Clínicas + Turnero Online + Cobro QR)
├── MANUAL_USUARIO.md     # Guía operativa para el odontólogo y recepción
└── MANUAL_TECNICO.md     # Documentación técnica, esquemas y API
```

---

## 2. Esquema de Datos (LocalStorage)

### 1. `dentista_settings`
```typescript
{
  salonName: string;          // "Odonto · Dr. Martín"
  salonLogo: string;          // Imagen comprimida en base64
  salonPhone: string;         // Teléfono oficial para turnos "54911..."
  paymentAlias: string;       // Alias Mercado Pago o CBU bancario
  paymentHolder: string;      // Titular de la cuenta
  template: string;           // Template con variables {nombre}, {servicio}, {semanas}
  bookingEnabled: boolean;    // Habilitación del turnero público
  bookingStartHour: number;   // Hora inicial de agenda (ej: 8)
  bookingEndHour: number;     // Hora final de agenda (ej: 19)
  workDays: number[];         // Array de días de atención [1, 2, 3, 4, 5]
  loyaltyEnabled: boolean;    // Programa de fidelización/beneficios
  loyaltyTarget: number;      // Meta de visitas
  loyaltyReward: string;      // Beneficio (ej: "Limpieza ultrasónica sin cargo")
}
```

### 2. `dentista_clients`
```typescript
Array<{
  id: string;                 // "c_1730000000000"
  name: string;               // Nombre del paciente
  phone: string;              // Número WhatsApp
  lastCutDate: string;        // Fecha de última consulta "YYYY-MM-DD"
  habitualService: string;    // Tratamiento habitual ("Limpieza", "Ortodoncia", etc.)
  notes: string;              // Antecedentes odontológicos o notas
  stamps: number;             // Visitas computadas
  totalVisits: number;        // Contador histórico
  debt: number;               // Saldo adeudado en ARS $ (0 = al día)
}>
```

---

## 3. Protocolo de Turnero en la Nube y Concurrencia

* **ID de Aplicación:** `CLOUD_APP_ID = 'dentista'`
* **Control de Concurrencia:**
  1. `fetchCloudDay(dateStr)` verifica atómicamente el estado del día antes de confirmar cualquier turno.
  2. Si `latestDay.hours.includes(hourKey)` o `latestDay.closed === true`, la reserva es denegada con mensaje informativo al paciente.
  3. Al confirmarse, el casillero se agrega al array y se persiste vía POST al endpoint cloud.
* **Control Manual del Profesional:**
  * El botón `openOwnerAgendaModal()` permite al profesional bloquear horarios ocupados en consultorio (`t: 'owner'`) o cerrar fechas completas.

---

## 4. Cobro QR y Seguridad

* **QR Fallback Engine:** Motor dual `QRCode.js` + fallback automático a la API de `QuickChart.io`.
* **Seguridad de Datos:** La vista de turnos para pacientes no expone datos médicos ni fichas clínicas de otros pacientes; solo muestra disponibilidad horaria (bloques libres / ocupados).
* **Formato Moneda y Horarios:** Todos los montos en moneda local (ARS $, separador de miles con punto) y reloj argentino GMT-3 de 24hs.
