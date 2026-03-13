# 🗓️ Tools Karen Trujillo — Admin PWA

Herramienta de gestión de agenda diseñada para uso interno de la **Psic. Karen Trujillo**. Permite visualizar, editar y exportar horarios de disponibilidad como póster de alta resolución listo para compartir.

---

## 📁 Estructura del proyecto

```
Tools-Karen-Trujilo/
├── index.html                  # Shell de navegación (home + menú)
├── herramienta-agenda.html     # Herramienta principal de agenda
└── Logo_Karen_Trujillo.webp    # Logo usado como watermark en el póster
```

> Los tres archivos deben estar en la **misma carpeta** para que la app funcione correctamente.

---

## ⚙️ Funcionamiento general

La app es un **SPA sin framework ni build step** — corre directo en el browser, sin instalación. `index.html` actúa como shell y carga `herramienta-agenda.html` dentro de un `<iframe>`.

### Flujo de uso

1. **Seleccionar rango de fechas** → la fecha de inicio es libre; la fecha fin se ajusta automáticamente al **sábado de esa misma semana** (los domingos no se contemplan).
2. **Sincronizar Google Calendar** (opcional) → jalona los eventos del rango desde un Google Apps Script.
3. **Editar manualmente** → agregar, eliminar o ajustar horas por día.
4. **Generar y compartir** → exporta el póster como PNG de 1080×1920px.

---

## 🕐 Horario base predeterminado

Al seleccionar cualquier rango, cada día se pre-llena automáticamente con el siguiente horario. Las sesiones son de **50 minutos** con **10 minutos de break** entre cada una (intervalo total: 60 min).

| Día | Horarios |
|-----|----------|
| Lunes y Martes | 10:00 AM, 11:00 AM, 12:00 PM, 1:00 PM, 2:00 PM, 6:00 PM |
| Miércoles y Jueves | 9:00 AM, 10:00 AM, 11:00 AM, 12:00 PM, 5:00 PM, 6:00 PM |
| Viernes | 10:00 AM, 11:00 AM, 12:00 PM, 1:00 PM |
| Sábado | 10:00 AM, 11:00 AM, 12:00 PM, 4:00 PM, 5:00 PM, 6:00 PM |
| Domingo | — (descanso, no aparece) |

> Si un día ya tiene datos guardados en `localStorage`, el horario base **no los sobreescribe**.

---

## ✏️ Edición manual

### Eliminar una hora
Presionar la **×** roja sobre cualquier chip.

### Agregar una hora
Presionar el botón **+** en la esquina del día. Acepta formato 12h (`3:00 PM`) o 24h (`15:00`).

### Editar una hora existente
Tocar el chip abre un selector de hora nativo. Al confirmar aparece un modal con dos opciones:

| Opción | Comportamiento |
|--------|----------------|
| **SÍ, RECORRER TODO** | Propaga el mismo desplazamiento a todas las citas posteriores del día |
| **NO, SOLO AJUSTAR SIGUIENTE** | Solo mueve la cita inmediatamente siguiente a `hora editada + 60 min` |

**Ejemplo:** Si se cambia `6:00 PM` → `6:30 PM` y se elige "NO":
- `6:30 PM` (editada)
- `7:30 PM` (siguiente ajustada automáticamente)

---

## 🔄 Sincronización con Google Calendar

Conectada vía **Google Apps Script**. Al sincronizar:
- Se sobreescriben todos los datos del rango actual.
- Los horarios importados se **redondean a la hora entera** (≥30 min sube al siguiente, <30 min trunca).
- Los duplicados se eliminan automáticamente.

La URL del script está hardcodeada en `herramienta-agenda.html`:
```js
const API = "https://script.google.com/macros/s/AKfycb.../exec";
```

---

## 💾 Persistencia

Toda la información se guarda en `localStorage` del navegador:

| Key | Contenido |
|-----|-----------|
| `karen_start` | Fecha de inicio del rango |
| `karen_end` | Fecha fin del rango |
| `karen_notes` | Notas del póster |
| `karen_agenda` | Objeto JSON con los horarios por fecha (`YYYY-MM-DD`) |

> Al usar **Resetear Todo** se limpia el `localStorage` completo.

---

## 🖼️ Exportación del póster

El póster se genera con `html2canvas` a **1080×1920px** (formato Stories/vertical) y se descarga como `Agenda_Karen_Trujillo.png`. Incluye:
- Rango de fechas
- Grilla de días con sus horarios disponibles
- Notas personalizadas
- Watermark con el logo al 10% de opacidad

---

## 🧰 Tech stack

| Tecnología | Uso |
|------------|-----|
| HTML + CSS vanilla | Estructura y estilos |
| Tailwind CDN | Utilidades CSS en `index.html` |
| Google Fonts (Montserrat + Playfair Display) | Tipografía |
| Font Awesome 6 | Iconos |
| html2canvas 1.4.1 | Generación del PNG |
| Google Apps Script | Backend de Google Calendar |
| localStorage | Persistencia local |

Sin dependencias npm. Sin build step. Sin framework.

---

## 🚀 Deploy

El proyecto está desplegado en **Vercel** bajo el dominio del sitio de Karen Trujillo. Al ser HTML estático puro, cualquier hosting de archivos estáticos funciona (Vercel, Netlify, GitHub Pages, etc.).
