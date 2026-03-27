# Gastos-familiares[README.md](https://github.com/user-attachments/files/26305658/README.md)
# 💰 Finanzas Vila — App + Google Sheets

Sistema de registro de gastos personales con app web móvil y planilla Google Sheets automática.

---

## ¿Qué hace?

- **App web** (abre desde el cel, sin instalar nada): formulario rápido para cargar gastos
- **Google Sheets** se actualiza automáticamente con cada gasto
- Las cuotas se distribuyen automáticamente en los meses correspondientes
- Muestra cuotas activas con barra de progreso y monto restante

---

## Configuración — 3 pasos

### Paso 1 — Crear el Google Sheet

1. Ir a [sheets.google.com](https://sheets.google.com) → Crear nueva hoja
2. Nombrarla `Finanzas 2026`
3. Copiar el **ID** de la URL:  
   `https://docs.google.com/spreadsheets/d/`**`ESTE_ES_EL_ID`**`/edit`

### Paso 2 — Configurar el Apps Script

1. Ir a [script.google.com](https://script.google.com) → **Nuevo proyecto**
2. Borrar el código que aparece por defecto
3. Pegar el contenido de [`Code.gs`](./Code.gs)
4. En la línea 9, reemplazar `PEGAR_ACÁ_EL_ID_DE_TU_GOOGLE_SHEET` por el ID copiado en el paso 1:
   ```js
   const SPREADSHEET_ID = "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms";
   ```
5. Guardar (Ctrl+S o Cmd+S)
6. Clic en **Implementar → Nueva implementación**
7. Tipo: **App web**
8. Ejecutar como: **Yo**
9. Quién tiene acceso: **Cualquier persona**
10. Clic en **Implementar** → Copiar la URL que aparece

> ⚠️ La primera vez pedirá permisos de Google. Aceptar todo.

### Paso 3 — Conectar la app web

**Opción A — GitHub Pages (recomendado):**

1. Crear un repositorio en GitHub (puede ser privado)
2. Subir el archivo `index.html` a la raíz
3. Ir a Settings → Pages → Source: `main` branch → `/root`
4. Abrir la URL que genera GitHub Pages desde el celular
5. Pegar la URL del Apps Script cuando lo pida

**Opción B — Abrir localmente:**
- Abrir `index.html` directamente en el navegador del celular o PC

---

## Cómo funciona la automatización de cuotas

Cuando cargás un gasto con 12 cuotas de $10.000:

```
Compra: Travega.com — $120.000 — 12 cuotas — VISA BNA

→ REGISTRO GASTOS recibe 12 filas:
   Abril 2026   · Cuota 1/12 · $10.000
   Mayo 2026    · Cuota 2/12 · $10.000
   Junio 2026   · Cuota 3/12 · $10.000
   ... y así hasta Marzo 2027

→ CUOTAS ACTIVAS recibe 1 fila:
   Travega.com | VISA BNA | Cuota 1 | 12 total | $10.000/mes | 12 pend. | $120.000 restante
```

---

## Estructura de Google Sheets

El script crea automáticamente las hojas que no existan:

| Hoja | Descripción |
|------|-------------|
| `REGISTRO GASTOS` | Cada gasto con sus cuotas distribuidas por mes |
| `CUOTAS ACTIVAS` | Lista de cuotas en curso con progreso |

> Las hojas `GASTOS FIJOS`, `TARJETAS` y `TABLERO` se mantienen del Excel original que ya tenés.

---

## Avanzar cuotas mensualmente

Una vez por mes, podés ejecutar la función `avanzarCuotas()` desde el editor de Apps Script para actualizar el contador de cuotas. También podés configurar un **trigger automático**:

1. En el editor de Apps Script → **Triggers** (reloj) → Agregar trigger
2. Función: `avanzarCuotas`
3. Tipo: **Timer mensual** → Día 1 de cada mes

---

## Agregar una segunda persona

En la app, el selector "Quién gasta" ya tiene `Vila Maria` y `Persona 2`.  
Para cambiar el nombre de "Persona 2":

1. Abrir `index.html` con un editor de texto
2. Buscar `Persona 2` (aparece 2 veces)
3. Reemplazar por el nombre real

En Google Sheets podés filtrar por la columna `PERSONA` para ver gastos individuales o totales del hogar.

---

## Tecnologías

- **Frontend**: HTML + CSS + JS vanilla (sin frameworks, carga instantánea)
- **Backend**: Google Apps Script (gratis, en tu cuenta Google)
- **Base de datos**: Google Sheets (podés editarlo directamente también)
- **Hosting**: GitHub Pages (gratis)

---

## Archivos

```
finanzas-app/
├── index.html   ← App web (subir a GitHub Pages)
├── Code.gs      ← Backend Google Apps Script
└── README.md    ← Este archivo
```
