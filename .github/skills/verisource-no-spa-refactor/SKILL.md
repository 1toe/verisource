---
name: verisource-no-spa-refactor
description: Complete refactoring guide to convert VeriSource Hub from a React SPA to a vanilla HTML/CSS/JS static site. Use when asked to refactor without SPA, remove React, convert to vanilla JavaScript, eliminate framework dependencies, create static site, or migrate away from React. Covers architecture mapping, data migration, i18n without React Context, theme system, search, favorites, history, AI integration, and Tailwind CSS standalone usage.
---

# VeriSource Hub — Refactorización sin SPA

Guía completa para transformar VeriSource Hub de una Single Page Application (React 18 + Vite) a un sitio estático con HTML/CSS/JS vanilla, manteniendo TODA la funcionalidad existente.

## Cuándo Usar Este Skill

- Refactorizar VeriSource Hub eliminando React y dependencias SPA
- Convertir componentes JSX a HTML/JS vanilla
- Migrar de React Context a un sistema de estado vanilla
- Eliminar Vite como bundler y usar Tailwind CSS standalone
- Crear una versión estática desplegable sin build de React
- Optimizar rendimiento eliminando overhead de framework

## Arquitectura Actual (SPA React)

### Stack Actual

| Tecnología | Uso | Se mantiene |
|-----------|-----|-------------|
| React 18 | UI Framework (SPA) | **NO** — se elimina |
| ReactDOM | Rendering | **NO** — se elimina |
| Vite | Bundler/Dev Server | **PARCIAL** — reemplazar por servidor estático |
| Tailwind CSS | Estilos | **SÍ** — usar CLI standalone |
| Lucide React | Iconos | **NO** — reemplazar por Lucide vanilla/SVG inline |
| PostCSS + Autoprefixer | CSS processing | **SÍ** — integrado con Tailwind CLI |
| ESLint | Linting | **OPCIONAL** — adaptar a JS vanilla |

### Estructura de Archivos Actual

```
src/
├── App.jsx                      # Componente monolítico (1238 líneas) — TODO el UI
├── main.jsx                     # Entry point React con LanguageProvider
├── index.css                    # Estilos globales + Tailwind directives
├── components/
│   ├── ExportTools.jsx          # Exportar favoritos, compartir config
│   └── SearchHistory.jsx        # Vista historial de búsquedas
├── data/
│   └── osint-data.jsx           # Catálogo OSINT (8 categorías, 56+ herramientas)
├── i18n/
│   ├── LanguageContext.jsx      # React Context para idiomas
│   └── translations.js          # Traducciones ES/EN completas
└── utils/
    ├── hooks.js                 # Custom hooks: useTheme, useFavorites, useSearchHistory, etc.
    ├── preload.js               # Estrategias de precarga (idle, hover, delay)
    └── theme.js                 # createTheme, buttonVariants, storage utils, debounce
```

### Funcionalidades a Preservar

1. **Búsqueda en tiempo real** — Filtrar herramientas por nombre, descripción, tags, categoría
2. **Categorías** — 8 categorías + vista "Todo" con sidebar de navegación
3. **Favoritos** — Toggle favoritos con persistencia localStorage
4. **Historial de búsquedas** — Últimas 20 búsquedas con timestamp y conteo de resultados
5. **Tema oscuro/claro** — Toggle con persistencia, detección sistema, clase `dark` en `<html>`
6. **Internacionalización** — ES/EN con detección de navegador y persistencia
7. **IA Gemini** — Plan de investigación y generador de dorks (textarea → API → resultado)
8. **Atajos de teclado** — Ctrl+K (buscar), Ctrl+Shift+F (favoritos), Ctrl+Shift+H (historial), Ctrl+Shift+D (tema), Escape (cerrar)
9. **Exportar datos** — JSON de favoritos, compartir via Web Share API
10. **Responsive** — Sidebar colapsable en móvil, grid adaptivo
11. **Tags por herramienta** — Color mapping para 80+ tags con estilos glassmorphism
12. **Animaciones** — fadeIn, slideUp, pulseGlow, float, card-hover

### Datos Clave

- **8 categorías**: searchEngines, socialMedia, personSearch, imageMapping, corporateTech, transportLive, artificialIntelligence, verificationBlogs
- **56+ herramientas OSINT** con: name, url, descKey (clave i18n), tags[]
- **2 idiomas**: Español (por defecto) e Inglés
- **API Gemini**: `gemini-2.5-flash-preview-09-2025` para AI features

## Prerequisitos

- Node.js 18+ (solo para Tailwind CLI durante desarrollo)
- Un servidor HTTP simple (Live Server, `npx serve`, Python `http.server`)
- API key de Google Gemini (opcional, para funciones IA)

## Flujo de Refactorización Paso a Paso

### Paso 1: Crear estructura de archivos estática

```
/ (raíz del proyecto)
├── index.html              # HTML principal con todo el markup
├── css/
│   ├── input.css           # Tailwind directives + custom CSS
│   └── output.css          # CSS compilado por Tailwind CLI
├── js/
│   ├── app.js              # Lógica principal y orquestación
│   ├── data.js             # Catálogo OSINT (export como objeto JS)
│   ├── i18n.js             # Sistema de traducciones vanilla
│   ├── theme.js            # Gestión de tema dark/light
│   ├── search.js           # Lógica de búsqueda y filtrado
│   ├── favorites.js        # Gestión de favoritos
│   ├── history.js          # Historial de búsquedas
│   ├── ai.js               # Integración Gemini API
│   ├── keyboard.js         # Atajos de teclado
│   └── storage.js          # Utilidades localStorage
├── icons/                  # SVGs de Lucide descargados
├── public/
│   └── favicon.svg
├── tailwind.config.js      # Configuración Tailwind (adaptar content paths)
└── package.json            # Solo scripts de Tailwind CLI
```

### Paso 2: Migrar Datos — ver [data-catalog.md](./references/data-catalog.md)

Convertir `osint-data.jsx` de exports JSX a objeto JavaScript puro:
- Eliminar imports de Lucide React
- Reemplazar `icon: <Component />` por `icon: "icon-name"` (string)
- Resolver SVGs por nombre en runtime

### Paso 3: Migrar Sistema i18n — ver [translations-reference.md](./references/translations-reference.md)

Reemplazar React Context por módulo vanilla:
- Función `t(key)` con dot-notation: `t('categories.searchEngines')`
- Detección de idioma del navegador
- Persistencia en localStorage
- Fallback automático a inglés

### Paso 4: Migrar Sistema de Tema

Convertir de `useState` + `useEffect` a vanilla:
- Toggle clase `dark` en `document.documentElement`
- localStorage para persistencia
- `matchMedia('(prefers-color-scheme: dark)')` para detección del sistema

### Paso 5: Migrar Búsqueda y Filtrado

Convertir `useMemo` de React a funciones puras:
- Debounce manual con `setTimeout`
- `renderTools(filteredData)` que actualiza el DOM
- Event listener en input con debounce

### Paso 6: Generar HTML estático

Ver [refactoring-plan.md](./references/refactoring-plan.md) para el HTML completo con:
- Header sticky con controles
- Sidebar con categorías
- Grid de herramientas
- Modal de AI
- Vistas de favoritos e historial

### Paso 7: Reemplazar Iconos Lucide

Opciones:
1. **Lucide vanilla**: `npm install lucide` y usar `createIcons()`
2. **SVGs inline**: Copiar SVGs directamente en el HTML
3. **Sprite SVG**: Un archivo sprite con `<use xlink:href="#icon-name">`

Recomendado: Lucide vanilla con `createIcons()` para menor mantenimiento.

### Paso 8: Configurar Tailwind CSS Standalone

```bash
# package.json mínimo
{
  "scripts": {
    "dev": "npx tailwindcss -i ./css/input.css -o ./css/output.css --watch",
    "build": "npx tailwindcss -i ./css/input.css -o ./css/output.css --minify"
  },
  "devDependencies": {
    "tailwindcss": "^3.3.3",
    "autoprefixer": "^10.4.14"
  }
}
```

Adaptar `tailwind.config.js`:
```js
content: [
  "./index.html",
  "./js/**/*.js",
]
```

## Mapeo de Componentes React → Vanilla

| React (Actual) | Vanilla (Nuevo) | Notas |
|----------------|-----------------|-------|
| `useState(initialValue)` | Variable + función `setState()` que re-renderiza | Usar closure o módulo |
| `useEffect(() => {}, [deps])` | `addEventListener` / `DOMContentLoaded` | Sin cleanup automático |
| `useMemo(() => {}, [deps])` | Función pura llamada según necesidad | Cache manual si es costoso |
| `useCallback(fn, [deps])` | Función normal (no hay re-renders) | Innecesario en vanilla |
| `useContext(LanguageContext)` | Módulo singleton `i18n.t('key')` | Import directo |
| JSX `<Component prop={x} />` | `document.createElement` / template literals | Preferir template literals |
| `className={...}` | `element.className = ...` | Mismo Tailwind |
| `onClick={handler}` | `element.addEventListener('click', handler)` | O atributo `onclick` |
| `onChange={(e) => ...}` | `input.addEventListener('input', handler)` | Usar `input` no `change` |
| React key en listas | No necesario | DOM vanilla no lo requiere |

## Patrón de Renderizado Recomendado

```javascript
// Patrón: Render functions con template literals
function renderToolCard(item, category, isFavorite) {
  return `
    <div class="group relative p-6 rounded-2xl border transition-all duration-300 card-hover
      ${isDarkMode 
        ? 'bg-slate-900/60 border-slate-800/50 hover:bg-slate-800/60' 
        : 'bg-white/90 border-slate-200 hover:bg-slate-50/90 shadow-sm'
      } hover:border-VeriSource-500/40 hover:shadow-xl"
      data-url="${item.url}" data-desc-key="${item.descKey}">
      <div class="flex justify-between items-start mb-4">
        <div class="flex-1 min-w-0">
          <h3 class="font-semibold text-lg truncate pr-4">${item.name}</h3>
          <div class="flex items-center gap-2 mt-2">
            ${item.tags.slice(0, 2).map(tag => 
              `<span class="px-2 py-1 rounded-md text-xs font-medium ${getTagColor(tag)}">${tag}</span>`
            ).join('')}
          </div>
        </div>
        <div class="flex items-center gap-2 shrink-0">
          <button onclick="toggleFavorite('${item.url}')" class="p-2 rounded-lg transition-all hover:scale-110">
            ${isFavorite ? '★' : '☆'}
          </button>
          <a href="${item.url}" target="_blank" rel="noopener noreferrer" class="p-2 rounded-lg">
            <!-- external-link SVG -->
          </a>
        </div>
      </div>
      <p class="text-sm line-clamp-2">${t('tools.' + item.descKey)}</p>
    </div>
  `;
}

// Render completo
function renderTools() {
  const filtered = filterData();
  const container = document.getElementById('tools-grid');
  container.innerHTML = filtered.map((cat, i) => `
    <section class="animate-slide-in-from-bottom" style="animation-delay: ${i * 100}ms">
      <div class="flex items-center gap-4 mb-8">
        <div class="p-3 rounded-xl bg-gradient-to-r ${cat.color} shadow-lg">
          ${getIconSVG(cat.icon)}
        </div>
        <h2 class="text-2xl font-bold">${t('categories.' + cat.category)}</h2>
      </div>
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        ${cat.items.map(item => renderToolCard(item, cat, isFavorite(item))).join('')}
      </div>
    </section>
  `).join('');
}
```

## Troubleshooting

| Problema | Solución |
|----------|----------|
| Tailwind no genera clases dinámicas | Usar safelist en config o clases completas (no interpolar) |
| SVGs de Lucide no aparecen | Verificar que `createIcons()` se ejecuta después de cada render |
| localStorage no persiste | Verificar que los valores se serializan/deserializan con JSON |
| Búsqueda lenta con muchos items | Implementar debounce de 300ms y cache de resultados |
| Tema parpadea al cargar (FOUC) | Ejecutar script de tema en `<head>` antes del CSS |
| Animaciones no funcionan | Asegurar que las clases de animación están en safelist |
| i18n no detecta idioma | Verificar `navigator.language` y fallback chain |

## Referencias

- [Arquitectura actual completa](./references/current-architecture.md)
- [Plan de refactorización detallado](./references/refactoring-plan.md)
- [Catálogo de datos OSINT](./references/data-catalog.md)
- [Traducciones completas ES/EN](./references/translations-reference.md)
