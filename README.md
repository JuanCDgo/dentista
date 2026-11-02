# 🦷 Manual Técnico: Micro App Odontología & Consultorio Dental

Aplicación web progresiva (PWA / SPA) ultra ligera construida en Vanilla JavaScript (ES6+), HTML5 y Tailwind CSS, diseñada para consultorios odontológicos, dentistas y ortodoncistas independientes en Argentina.

---

## 🏗️ 1. Arquitectura y Tecnologías
- **Frontend SPA sin build step:** Cero bundlers, ejecución nativa en cualquier navegador moderno o Webview (Android / iOS).
- **Estilos:** Tailwind CSS (CDN oficial) con paleta médica personalizada (`clinical` teal `#14B8A6/#0D9488` y `medicalSky` `#0284C7`).
- **Librería QR local:** `qrcode.min.js` (empaquetado localmente para funcionamiento offline y fallback a QuickChart API si fuera necesario).
- **Confeti nativo:** Animación por Canvas HTML5 (`launchCardConfetti`) sin dependencias de terceros.
- **Persistencia:** LocalStorage (`dentista_clients`, `dentista_settings`) con sincronización bidireccional automática a Google Drive mediante Google Apps Script Webhook.
- **Protocolo WhatsApp seguro:** Enlace universal `https://wa.me/${phone}?text=${encodeURIComponent(msg)}` con tipografía limpia y caracteres compatibles con Windows WhatsApp Desktop (sin emojis de 4 bytes que generen el error de rombo de reemplazo ``).

---

## 💾 2. Modelo de Datos (LocalStorage)

### Paciente (`clients` en `dentista_clients`)
```typescript
interface Client {
  id: string;                  // Timestamp ID único
  name: string;                // Nombre y Apellido
  phone: string;               // Celular con código de área (ej: 54911...)
  treatment: string;           // 'Ortodoncia' | 'Limpieza y Control' | 'Conducto / Endodoncia' | ...
  toothArea: string;           // Ej: 'Pieza 16', 'Sector anterior'
  medicalAlert: string;        // Alergias o condiciones (ej: 'Penicilina')
  lastVisit: string;           // Fecha ISO (YYYY-MM-DD)
  intervalWeeks: number;       // Semanas de intervalo (ej: 4 para ortodoncia, 26 para control)
  debt: number;                // Saldo pendiente en ARS
  notes: string;               // Historia clínica / evolución
  stamps: number;              // Controles preventivos cumplidos (0 a N)
  lastReminderSent: string | null; // ISO timestamp del último WhatsApp enviado
}
```

### Configuración (`settings` en `dentista_settings`)
```typescript
interface Settings {
  salonName: string;           // Nombre del consultorio / odontólogo
  salonLogo: string;           // Base64 DataURL
  template: string;            // Plantilla de recordatorio
  driveWebhookUrl: string;     // Webhook de Google Apps Script
  driveFolderName: string;    // 'Copia de seguridad Dentistas'
  autoSyncDrive: boolean;      // Sincronización automática
  bookingEnabled: boolean;     // Switch turnero online
  salonPhone: string;          // Celular para recibir turnos y canjes
  workDays: number[];          // Días laborales (0=Dom, 1=Lun, ..., 6=Sáb)
  bookingStartHour: number;    // Hora inicio (ej: 9)
  bookingEndHour: number;      // Hora fin (ej: 19)
  bookingSlotMinutes: number;  // Duración del turno (ej: 45 min)
  paymentAlias: string;        // Alias bancario / MP (ej: 'odonto.martin.mp')
  paymentHolder: string;       // Titular de la cuenta
  loyaltyEnabled: boolean;     // Switch Programa de Prevención
  loyaltyTarget: number;       // Meta de controles (default: 3)
  loyaltyReward: string;       // Beneficio (default: 'Limpieza Profunda y Profilaxis Sin Cargo')
}
```

---

## 💳 3. Módulos Nuevos Integrados

### 1. Cobro con QR en Mostrador (`businessQrModal`)
- **Pestaña Cobro por Transferencia:**
  - Monto dinámico en `$ ARS` con botones rápidos (`+$5.000`, `+$10.000`, `+$20.000`, `Limpiar`).
  - Si se abre desde la ficha de un paciente, precarga automáticamente su saldo adeudado.
  - Generación de código QR en tiempo real para escaneo con apps bancarias y Mercado Pago.
  - Copia rápida de Alias y Titular en 1 toque.
  - Botón **"Marcar Saldo como Cobrado ✓"** que liquida la deuda del paciente y actualiza métricas inmediatamente.
- **Pestaña QR de Turnero:**
  - Código QR de mostrador para que el paciente escanee con su celular en recepción y solicite su turno online.

### 2. Programa de Prevención Bucal & Fidelización (`clientLoyaltyModal` y `clientLoyaltyView`)
- **Lógica de Salud:** Basada en controles periódicos semestrales (Opción A: 3 controles = beneficio preventivo como Profilaxis profunda sin cargo).
- **Gestión Odontológica:** Stepper rápido `+ Sumar Control 🦷` y `- Restar`, canje con reinicio de ciclo y botón para enviar el pase al paciente vía WhatsApp.
- **Acceso Directo:** El botón `🦷 Vino hoy` incrementa automáticamente el control del paciente y reinicia el contador de semanas.
- **Pase Digital del Paciente (`?fidelidad=1`):**
  - Vista exclusiva responsive para el móvil del paciente.
  - Cuadrícula de controles dentales con íconos `🦷`.
  - Barra de progreso porcentual.
  - Animación de confeti al completar el objetivo.
  - Botón para canjear premio por WhatsApp y agendar el próximo control.

---

## 🌐 4. URL de Producción en GitHub Pages
👉 **https://juancdgo.github.io/dentista/**
