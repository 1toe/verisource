# Arquitectura Actual — VeriSource Hub (React SPA)

## Visión General

VeriSource Hub es una Single Page Application construida con React 18 + Vite que sirve como directorio de herramientas OSINT (Open Source Intelligence). La aplicación NO usa routing — toda la UI vive en un solo componente `App.jsx` de 1238 líneas.

## Dependencias de Producción

```json
{
  "react": "^18.2.0",
  "react-dom": "^18.2.0",
  "lucide-react": "^0.263.1",
  "vercel": "^50.15.0"
}
```

## Dependencias de Desarrollo

```json
{
  "@vitejs/plugin-react": "^4.0.3",
  "tailwindcss": "^3.3.3",
  "postcss": "^8.4.27",
  "autoprefixer": "^10.4.14",
  "vite": "^7.3.1",
  "eslint": "^8.45.0"
}
```

## Entry Point: `main.jsx`

```jsx
import { StrictMode } from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx';
import './index.css';
import { LanguageProvider } from './i18n/LanguageContext.jsx';

const initializeTheme = () => {
  const savedTheme = localStorage.getItem('VeriSource-hub-theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  if (savedTheme) {
    document.documentElement.classList.toggle('dark', savedTheme === 'dark');
  } else {
    document.documentElement.classList.toggle('dark', prefersDark);
    localStorage.setItem('VeriSource-hub-theme', prefersDark ? 'dark' : 'light');
  }
};

initializeTheme();

ReactDOM.createRoot(document.getElementById('root')).render(
  <StrictMode>
    <LanguageProvider>
      <App />
    </LanguageProvider>
  </StrictMode>
);
```

**Equivalente vanilla:** Ejecutar `initializeTheme()` en `<head>` y inicializar módulos en `DOMContentLoaded`.

## Componente Principal: `App.jsx` (1238 líneas)

### Estado Global (useState)

| Variable | Tipo | Default | Persistencia |
|----------|------|---------|-------------|
| `osintData` | Array\|null | null (lazy load) | No |
| `searchQuery` | string | "" | No |
| `activeCategory` | string | "all" | No |
| `isSidebarOpen` | boolean | false | No |
| `isDarkMode` | boolean | true (o localStorage) | `osint-hub-theme` |
| `favorites` | Array | [] (o localStorage) | `VeriSource-hub-favorites` |
| `searchHistory` | Array | [] (o localStorage) | `VeriSource-hub-search-history` |
| `showFavorites` | boolean | false | No |
| `showHistory` | boolean | false | No |
| `isAiActive` | boolean | false | No |
| `aiInput` | string | "" | No |
| `aiOutput` | string | "" | No |
| `aiLoading` | boolean | false | No |
| `aiMode` | string | "plan" | No |

### Datos Embebidos en App.jsx

El componente `App.jsx` contiene una copia completa de `VeriSource_DATA` (idéntica a `osint-data.jsx`) directamente en el archivo. Esto es redundancia y debe consolidarse en la versión vanilla.

### Funciones Principales

```
toggleFavorite(tool)      → Toggle favorito + persistir en localStorage
addToSearchHistory(q, r)  → Agregar búsqueda al historial (max 20)
clearSearchHistory()      → Limpiar historial
searchFromHistory(query)  → Restaurar búsqueda desde historial
callGemini(prompt, sys)   → Llamada a Gemini API con retry (3 intentos, backoff exponencial)
handleAiAction()          → Orquestar llamada AI según modo (plan/dork)
filteredData (useMemo)    → Filtrar datos por categoría, búsqueda, favoritos
getTagColor(tag)          → Mapeo de 80+ tags a clases Tailwind
```

### Efectos (useEffect)

1. **Lazy load data**: `import('./data/osint-data')` al montar
2. **Keyboard shortcuts**: Registrar listeners globales
3. **Theme sync**: Sincronizar clase `dark` con localStorage

### Vistas del Main Content

La UI tiene 4 vistas exclusivas renderizadas condicionalmente:

1. **showFavorites** → Grid de herramientas favoritas
2. **showHistory** → Componente `SearchHistory`
3. **!isAiActive** → Vista principal: búsqueda + grid de categorías
4. **isAiActive** → Vista AI: selector de modo + textarea + resultados

## Componente: `ExportTools.jsx`

Funcionalidades:
- `exportFavorites()`: Genera y descarga JSON con favoritos
- `shareConfig()`: Usa Web Share API (con fallback a clipboard)

**Nota**: Este componente NO está siendo renderizado actualmente en App.jsx. Existe pero no se importa/usa.

## Componente: `SearchHistory.jsx`

Props: `{ history, onClearHistory, onSearchFromHistory, isDarkMode }`

Renderiza lista de búsquedas recientes con:
- Icono de reloj
- Query truncada
- Fecha formateada + conteo de resultados
- Botón "Limpiar" historial
- Click para restaurar búsqueda

## Sistema i18n: `LanguageContext.jsx`

React Context con:
- `currentLanguage`: string (es/en)
- `changeLanguage(lang)`: setter + localStorage + `document.documentElement.lang`
- `t(key)`: resolver clave dot-notation con fallback a inglés
- `availableLanguages`: array de idiomas disponibles

Detección automática:
1. localStorage `VeriSource-hub-language`
2. `navigator.language.split('-')[0]`
3. Fallback: 'es'

## Utilidades: `theme.js`

Funciones exportadas:
- `createTheme(isDarkMode)` → Objeto con clases Tailwind para cada elemento
- `buttonVariants` → Generadores de clases para botones (primary, icon, themed)
- `storage` → Wrapper de localStorage: `get(key, default)`, `set(key, value)`, `remove(key)`
- `STORAGE_KEYS` → Constantes para claves de localStorage
- `debounce(func, wait)` → Implementación estándar de debounce

## Utilidades: `hooks.js`

Custom hooks (cada uno debe convertirse a módulo vanilla):
- `useTheme()` → Toggle dark mode con persistencia
- `useFavorites()` → CRUD favoritos con localStorage
- `useSearchHistory()` → Historial con debounce
- `useKeyboardShortcuts(handlers)` → Registro de atajos
- `useDebouncedSearch(initial, delay)` → Búsqueda con debounce

**Nota**: Estos hooks NO están siendo usados en App.jsx — App.jsx reimplementa la lógica directamente con useState/useEffect.

## Utilidades: `preload.js`

Estrategias de precarga:
- `preloadComponent(import)` → Usar `requestIdleCallback` o fallback `setTimeout`
- `preloadStrategies.onHover` → Precargar al hover
- `preloadStrategies.onIdle` → Precargar cuando el navegador está idle
- `preloadStrategies.afterDelay` → Precargar después de X ms

**En vanilla**: No necesarias ya que no hay lazy loading de componentes React.

## Configuración Vite

```javascript
export default defineConfig({
  plugins: [react()],
  resolve: { alias: { '@': './src' } },
  server: { port: 3000, host: true },
  build: {
    outDir: 'dist',
    minify: 'esbuild',
    target: 'esnext',
    rollupOptions: {
      output: {
        manualChunks: { 'react-vendor': [...], 'icons': [...] }
      }
    }
  }
});
```

**En vanilla**: Reemplazar por Tailwind CLI + servidor estático.

## HTML Shell: `index.html`

Contiene:
- Meta tags SEO
- Google Fonts preconnect (Inter, Fira Code)
- Loading screen inline con CSS animation
- `<div id="root">` como mount point de React
- Script que oculta loading screen al cargar

**En vanilla**: El HTML será el archivo principal con TODO el markup estático.

## Tailwind Config

Configuración extendida con:
- Fuentes: Inter, Fira Code, Cal Sans
- Colores custom: `VeriSource` (azules), `cyber` (verdes)
- Animaciones: fadeIn, slideUp, pulseGlow, float
- Dark mode: class-based (`darkMode: 'class'`)

## Claves localStorage

| Clave | Contenido |
|-------|-----------|
| `osint-hub-theme` | `"dark"` o `"light"` |
| `VeriSource-hub-favorites` | JSON array de `{name, url, descKey, tags}` |
| `VeriSource-hub-search-history` | JSON array de `{query, results, timestamp}` |
| `VeriSource-hub-language` | `"es"` o `"en"` |

## API Gemini

- Endpoint: `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent`
- Auth: API key como query param
- Retry: 3 intentos con backoff exponencial (500ms, 1s, 2s)
- Modos:
  - `plan`: System prompt para plan de investigación OSINT
  - `dork`: System prompt para generar Google Dorks

## Íconos Lucide Usados

Los siguientes iconos de Lucide se usan en la aplicación:

```
Search, ExternalLink, Globe, Shield, User, Map, Image, Database, Camera,
MessageSquare, Activity, Briefcase, Lock, Cpu, FileText, CheckCircle,
ChevronRight, Menu, X, Anchor, Cloud, Zap, Sparkles, Loader2, Terminal,
Send, Sun, Moon, Languages, Star, StarOff, Download, Filter, TrendingUp,
Eye, Compass, Target, History, Heart, Clock, Trash2, Share2
```

Total: ~40 iconos distintos.
