# 📈 Depot Tracker

Herramienta personal para seguimiento de portafolios de acciones y ETFs. Un único archivo HTML que funciona directamente en el navegador — sin instalación, sin servidor, sin dependencias.

**[→ Ver demo en vivo](https://tu-usuario.github.io/depot-tracker)**

![screenshot](https://img.shields.io/badge/estado-activo-10b981?style=flat-square) ![html](https://img.shields.io/badge/tecnología-HTML%20%2F%20JS-3b82f6?style=flat-square) ![licencia](https://img.shields.io/badge/licencia-MIT-f59e0b?style=flat-square)

---

## ✦ Funcionalidades

| Función | Descripción |
|---|---|
| 📊 Dashboard en tiempo real | KPIs, gráfico de barras diario y distribución del portafolio |
| 💹 Precios automáticos | Conecta con Yahoo Finance vía proxy CORS público |
| 🔔 Alertas de precio | Notificaciones en el navegador cuando un ticker sube/baja de un umbral |
| 🤖 Análisis con IA | Integración con Claude (Anthropic) para análisis general, riesgo, rebalanceo y contexto macro |
| ⬇ Exportar a Excel | Genera un `.xlsx` descargable con todos los datos en un clic |
| 💾 Persistencia local | Los datos se guardan en `localStorage` — sobreviven al cerrar el navegador |

---

## 🚀 Cómo usar

### Opción A — Abrir localmente

1. Descarga `index.html`
2. Haz doble clic → se abre en tu navegador
3. Pulsa **↻ Actualizar** para cargar precios reales

### Opción B — GitHub Pages (recomendado)

Si hiciste fork de este repositorio, la app ya está publicada en:
```
https://TU-USUARIO.github.io/depot-tracker
```
Accesible desde cualquier dispositivo sin instalar nada.

---

## ⚙️ Configuración

### Cambiar el portafolio de ejemplo

Edita el array `portfolio` al inicio del `<script>` en `index.html`:

```javascript
let portfolio = [
  { ticker: 'AAPL',  shares: 10, costBasis: 150.00, price: null, change: null },
  { ticker: 'MSFT',  shares: 5,  costBasis: 280.00, price: null, change: null },
  // → añade tus propias posiciones aquí
];
```

Cada objeto representa una posición con estos campos:

| Campo | Tipo | Descripción |
|---|---|---|
| `ticker` | string | Símbolo bursátil (ej: `"NVDA"`, `"VOO"`) |
| `shares` | number | Cantidad de acciones que posees |
| `costBasis` | number | Precio promedio de compra en USD |
| `price` | number\|null | Precio actual (se rellena automáticamente) |
| `change` | number\|null | Cambio % del día (se rellena automáticamente) |

### Usar la IA fuera de Claude.ai

El análisis con IA funciona automáticamente dentro de [claude.ai](https://claude.ai). Si abres el archivo localmente, descomenta tu API key en la función `runAIAnalysis()`:

```javascript
headers: {
  'Content-Type': 'application/json',
  'x-api-key': 'sk-ant-TU_API_KEY_AQUI',  // ← descomenta esta línea
},
```

Obtén tu API key en [console.anthropic.com](https://console.anthropic.com).

> ⚠️ **Importante:** nunca subas tu API key a GitHub. Usa variables de entorno o un backend si necesitas desplegarla públicamente.

### Alertas por email (Make.com / n8n)

1. Pulsa **"Generar webhook"** en el panel de alertas
2. Copia la URL generada
3. En Make.com: crea un escenario → módulo **Webhooks → Custom webhook** → pega la URL
4. Conecta un módulo de Gmail/Outlook al final del escenario
5. En `index.html`, reemplaza la línea del `showToast` en `checkAlerts()` por:

```javascript
fetch('https://hook.make.com/TU-WEBHOOK', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ ticker: a.ticker, price: h.price, target: a.price, type: a.type })
});
```

---

## 🗂 Estructura del archivo

El proyecto es intencionalmente un único archivo para simplificar el despliegue:

```
index.html
│
├── <head>
│   ├── Chart.js (CDN)     → gráficos
│   └── SheetJS (CDN)      → exportar Excel
│
├── <style>
│   ├── Variables CSS      → paleta de colores
│   ├── Layout             → header, grid, paneles
│   └── Componentes        → tabla, alertas, modal, toast
│
├── <body>
│   ├── Header             → botones de acción
│   ├── Barra de estado    → indicador live/demo
│   ├── KPI Cards          → 4 métricas principales
│   ├── Gráficos           → barras + dona
│   ├── Tabla              → posiciones detalladas
│   ├── Alertas            → configuración de alertas
│   ├── Panel IA           → análisis Claude
│   └── Modal              → añadir posición
│
└── <script>
    ├── Estado global      → portfolio[], alerts[], save()
    ├── Yahoo Finance      → fetchPrice(), refreshAll()
    ├── Renderizado        → renderAll(), renderKPIs(), renderTable(), renderCharts()
    ├── Alertas            → addAlert(), checkAlerts(), removeAlert()
    ├── Holdings           → addHolding(), removeHolding()
    ├── Excel              → exportExcel()
    ├── IA                 → runAIAnalysis()
    └── Utils              → showToast(), fmt(), fmtUSD(), calcTotals()
```

---

## 🛠 Posibles mejoras

Ideas para extender el proyecto:

- [ ] Gráfico histórico de precios (requiere más llamadas a Yahoo Finance)
- [ ] Soporte para múltiples portafolios / cuentas
- [ ] Cálculo de dividendos recibidos
- [ ] Importar posiciones desde CSV (de Interactive Brokers, DEGIRO, etc.)
- [ ] Modo oscuro / claro toggle
- [ ] Notificaciones push del navegador (Web Push API)
- [ ] Backend con Python/Node para alertas por email sin depender de Make.com

---

## 🧰 Tecnologías

- **HTML / CSS / JavaScript** — sin frameworks, sin npm, sin build step
- **[Chart.js 4.4](https://www.chartjs.org/)** — gráficos
- **[SheetJS](https://sheetjs.com/)** — exportar Excel
- **[Yahoo Finance API](https://finance.yahoo.com/)** — precios en tiempo real (no oficial)
- **[Claude API](https://www.anthropic.com/)** — análisis con IA

---

## 📄 Licencia

MIT — úsalo, modifícalo y distribúyelo libremente.
