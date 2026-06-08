# MediSync — Manual de Uso

**Sistema Inteligente de Gestión y Seguimiento del Historial Clínico — Clínica Granados**
Proyecto de la asignatura Ingeniería de Software · Facultad de Sistemas y Telecomunicaciones · UPSE

---

## 1. ¿Qué es MediSync?

MediSync es un prototipo funcional (aplicación web/móvil de una sola página) que centraliza el historial clínico de los pacientes, automatiza recordatorios de medicamentos y citas, y permite al personal médico hacer seguimiento de la adherencia a los tratamientos. El sistema funciona con **control de acceso por roles** (paciente, médico y administrador), cada uno con su propio panel.

El archivo principal es `medisync.html`. Es autónomo: contiene HTML, CSS y JavaScript en un solo archivo, por lo que no requiere instalación de programas adicionales para probarse.

---

## 2. Requisitos

- Un navegador web moderno (Chrome, Edge, Firefox o Safari actualizados).
- Conexión a internet (solo para cargar las fuentes tipográficas; el resto funciona sin conexión en modo demo).
- Opcional: una cuenta de Google y una hoja de cálculo de Google Sheets si se desea **persistir los datos** (ver sección 7).

No se necesita servidor ni base de datos para la demostración: el sistema arranca en **modo demo** con datos de ejemplo precargados.

---

## 3. Cómo abrir la aplicación

1. Descarga el archivo `medisync.html`.
2. Haz doble clic sobre él, o ábrelo desde el navegador con `Archivo → Abrir`.
3. Verás la pantalla de inicio de sesión.

En la esquina inferior del panel de acceso aparece un indicador de estado:

- **"Modo demo — datos locales (no persiste)"**: el sistema funciona con datos de ejemplo. Todo lo que registres se borra al recargar la página.
- **"Conectado a Google Sheets"**: el sistema guarda los datos de forma permanente (requiere configuración previa, sección 7).

---

## 4. Inicio de sesión y registro

### 4.1. Cuentas de demostración

Selecciona el rol y usa una de estas cuentas. La contraseña para todas es **`demo1234`**.

| Rol | Correo | Qué verás |
|-----|--------|-----------|
| Paciente | `maria.gomez@granados.med.ec` | Recordatorios, historial propio y citas |
| Médico | `c.reyes@granados.med.ec` | Pacientes, seguimiento, citas y reportes |
| Administrador | `admin@granados.med.ec` | Reportes, gestión de usuarios y citas |

> Al elegir un rol con los botones **Paciente / Médico / Admin**, el correo de ejemplo se completa automáticamente.

### 4.2. Crear una cuenta nueva

1. En la pantalla de inicio, pulsa **"Regístrate aquí"**.
2. Elige el tipo de cuenta: **Paciente** o **Médico**.
3. Completa nombre, correo y contraseña (mínimo 4 caracteres). Según el tipo:
   - **Paciente**: cédula, edad y condiciones (opcionales).
   - **Médico**: especialidad (obligatoria).
4. Pulsa **"Crear cuenta e ingresar"**. El sistema te lleva directamente a tu panel.

---

## 5. Paneles según el rol

### 5.1. Paciente
- **Inicio (Resumen):** medicamentos del día, próximas citas, adherencia mensual y última toma de presión.
- **Historial Clínico:** línea de tiempo con diagnósticos, notas y medicamentos. Las entradas marcadas como alerta se resaltan.
- **Recordatorios:** lista de medicamentos del día; marca cada uno con el botón ✓ a medida que los tomas. El número en rojo del menú indica los pendientes.
- **Citas:** consulta tus citas, agéndalas o cancélalas.

### 5.2. Médico
- **Inicio:** indicadores de pacientes activos, citas confirmadas, alertas de adherencia y agenda del día.
- **Seguimiento:** registra pacientes, prescribe medicamentos y monitorea la adherencia. Los pacientes con adherencia menor al 70% se marcan en alerta.
- **Historial:** selecciona cualquier paciente (menú desplegable) y revisa o amplía su historial.
- **Citas:** agenda, confirma o cancela citas de cualquier paciente.
- **Reportes:** estadísticas de cumplimiento de citas y medicamentos más prescritos.

### 5.3. Administrador
- **Inicio:** métricas globales del sistema (pacientes, citas, cancelaciones, médicos).
- **Reportes:** gráficos de cumplimiento y adherencia, con opción de exportar.
- **Usuarios:** alta y baja de médicos y pacientes (control de acceso por roles - RBAC).
- **Citas:** vista global de la gestión de citas.

---

## 6. Acciones principales

| Acción | Cómo hacerla |
|--------|--------------|
| Marcar un medicamento como tomado | Panel del paciente → botón ✓ junto al medicamento |
| Agendar una cita | Botón **"Agendar cita"** → completar paciente, médico, fecha y hora |
| Confirmar / cancelar una cita | En la tarjeta de la cita, botones **Confirmar** o **Cancelar** (médico/admin) |
| Prescribir un medicamento | Médico → **Seguimiento** o **Historial** → **Prescribir** |
| Registrar un paciente | Médico/Admin → **Registrar paciente** |
| Registrar un médico | Admin → **Usuarios** → **Nuevo médico** |
| Cerrar sesión | Ícono de salida junto al nombre, en la parte inferior del menú lateral |

### Uso en dispositivos móviles
En pantallas pequeñas, el menú lateral se oculta. Pulsa el botón **☰** (arriba a la izquierda) para abrirlo; se cierra solo al elegir una opción o al tocar fuera de él.

---

## 7. Conectar con Google Sheets (datos permanentes) — opcional

Por defecto el sistema funciona en modo demo y **no guarda los cambios**. Para que los datos persistan:

1. Crea una hoja de cálculo en **Google Sheets** con las pestañas: `usuarios`, `pacientes`, `medicos`, `citas`, `prescripciones`, `historial`, usando las mismas columnas en español que maneja el sistema (id, nombre, correo, rol, etc.).
2. En Google Sheets ve a **Extensiones → Apps Script** y publica un script web (Web App) que responda a las acciones `getAll`, `login`, `addPatient`, `addDoctor`, `addAppointment`, etc. La URL publicada debe terminar en **`/exec`**.
3. Abre `medisync.html` en un editor de texto y busca esta línea cerca del inicio del bloque `Store`:

   ```js
   const CONFIG = { API_URL:'' };
   ```

4. Pega tu URL entre las comillas:

   ```js
   const CONFIG = { API_URL:'https://script.google.com/macros/s/XXXX/exec' };
   ```

5. Guarda y vuelve a abrir el archivo. El indicador debería mostrar **"Conectado a Google Sheets"**.

> Si la conexión falla, el sistema vuelve automáticamente al modo demo para que la aplicación siga siendo usable.

---

## 8. Estructura del proyecto

```
/
├── medisync.html              ← Aplicación (abre este archivo)
├── README.md                  ← Este manual de uso
└── PROYECTO_INGENIERIA.docx   ← Documento del plan inicial del proyecto
```

---

## 9. Solución de problemas

| Problema | Solución |
|----------|----------|
| No aparece el menú en el celular | Pulsa el botón **☰** arriba a la izquierda |
| Los datos se borran al recargar | Es normal en modo demo; configura Google Sheets (sección 7) para persistir |
| "Credenciales inválidas" | Verifica el correo y usa la contraseña `demo1234`; en modo demo se acepta cualquier correo registrado |
| Las fuentes se ven distintas | Revisa tu conexión a internet (las tipografías se cargan en línea) |
| No puedo confirmar una cita | Solo los roles **médico** y **administrador** pueden confirmar citas |

---

## 10. Notas del proyecto

Este prototipo corresponde al **primer parcial** de la asignatura, cuyo enfoque es la **planificación y documentación inicial**. La funcionalidad mostrada es una demostración de los seis módulos definidos en el plan (`PROYECTO_INGENIERIA.docx`): Gestión de Usuarios y Accesos, Historial Clínico Digital, Recordatorios Inteligentes, Gestión de Citas Médicas, Seguimiento de Tratamientos, y Reportes y Estadísticas.

Metodología aplicada: **Scrum** (8 sprints, 16 semanas).
