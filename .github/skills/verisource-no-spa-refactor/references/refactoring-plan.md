# Plan de Refactorización Detallado — React SPA → Vanilla HTML/CSS/JS

## Fase 1: Preparación del Proyecto

### 1.1 Nueva Estructura de Archivos

```
verisource/
├── index.html                # Página principal con markup completo
├── css/
│   ├── input.css             # Tailwind directives + CSS custom
│   └── output.css            # Generado por Tailwind CLI (no commitear)
├── js/
│   ├── app.js                # Orquestador principal
│   ├── data.js               # Catálogo OSINT como objeto JS puro
│   ├── i18n.js               # Sistema de traducción vanilla
│   ├── theme.js              # Tema dark/light
│   ├── search.js             # Búsqueda y filtrado
│   ├── favorites.js          # Gestión de favoritos
│   ├── history.js            # Historial de búsquedas
│   ├── ai.js                 # Integración Gemini API
│   ├── keyboard.js           # Atajos de teclado
│   ├── storage.js            # Wrapper localStorage
│   ├── icons.js              # SVGs inline o Lucide vanilla
│   └── render.js             # Funciones de renderizado DOM
├── public/
│   └── favicon.svg
├── tailwind.config.js        # Adaptado (sin JSX en content)
├── postcss.config.js         # PostCSS config
└── package.json              # Solo Tailwind CLI como dep
```

### 1.2 package.json Mínimo

```json
{
  "name": "verisource-hub",
  "version": "2.0.0",
  "private": true,
  "description": "Hub de recursos VeriSource - versión estática",
  "scripts": {
    "dev": "npx tailwindcss -i ./css/input.css -o ./css/output.css --watch",
    "build": "npx tailwindcss -i ./css/input.css -o ./css/output.css --minify",
    "serve": "npx serve ."
  },
  "devDependencies": {
    "tailwindcss": "^3.3.3",
    "autoprefixer": "^10.4.14"
  }
}
```

### 1.3 tailwind.config.js Adaptado

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./js/**/*.js",
  ],
  darkMode: 'class',
  theme: {
    extend: {
      fontFamily: {
        'sans': ['Inter', 'system-ui', 'sans-serif'],
        'mono': ['Fira Code', 'monospace'],
        'display': ['Cal Sans', 'Inter', 'sans-serif'],
      },
      colors: {
        'VeriSource': {
          50: '#f0f9ff', 100: '#e0f2fe', 200: '#bae6fd', 300: '#7dd3fc',
          400: '#38bdf8', 500: '#0ea5e9', 600: '#0284c7', 700: '#0369a1',
          800: '#075985', 900: '#0c4a6e',
        },
        'cyber': {
          50: '#f0fdf4', 100: '#dcfce7', 200: '#bbf7d0', 300: '#86efac',
          400: '#4ade80', 500: '#22c55e', 600: '#16a34a', 700: '#15803d',
          800: '#166534', 900: '#14532d',
        }
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.4s ease-out',
        'pulse-glow': 'pulseGlow 2s infinite',
        'float': 'float 3s ease-in-out infinite',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0', transform: 'translateY(10px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
        slideUp: {
          '0%': { opacity: '0', transform: 'translateY(20px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
        pulseGlow: {
          '0%, 100%': { boxShadow: '0 0 20px rgba(14, 165, 233, 0.4)' },
          '50%': { boxShadow: '0 0 30px rgba(14, 165, 233, 0.7)' },
        },
        float: {
          '0%, 100%': { transform: 'translateY(0px)' },
          '50%': { transform: 'translateY(-10px)' },
        }
      },
      backgroundImage: {
        'gradient-radial': 'radial-gradient(var(--tw-gradient-stops))',
      }
    },
  },
  plugins: [],
};
```

## Fase 2: Módulos JavaScript

### 2.1 `js/storage.js` — Utilidades localStorage

```javascript
// storage.js — Wrapper seguro de localStorage
export const KEYS = {
  THEME: 'osint-hub-theme',
  FAVORITES: 'VeriSource-hub-favorites',
  SEARCH_HISTORY: 'VeriSource-hub-search-history',
  LANGUAGE: 'VeriSource-hub-language',
};

export function get(key, fallback = null) {
  try {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : fallback;
  } catch {
    return fallback;
  }
}

export function set(key, value) {
  try {
    localStorage.setItem(key, JSON.stringify(value));
  } catch { /* quota exceeded */ }
}

export function remove(key) {
  try {
    localStorage.removeItem(key);
  } catch { /* noop */ }
}
```

### 2.2 `js/i18n.js` — Sistema de Traducción

```javascript
// i18n.js — Traducciones sin React Context
import { KEYS, get, set } from './storage.js';
import { translations } from './translations-data.js'; // Extraer de translations.js

let currentLang = 'es';

export function initI18n() {
  const stored = get(KEYS.LANGUAGE);
  const browserLang = navigator.language.split('-')[0];
  
  if (stored && translations[stored]) {
    currentLang = stored;
  } else if (translations[browserLang]) {
    currentLang = browserLang;
  } else {
    currentLang = 'es';
  }
  
  document.documentElement.lang = currentLang;
}

export function t(key) {
  const keys = key.split('.');
  let value = translations[currentLang];
  for (const k of keys) {
    value = value?.[k];
  }
  if (!value) {
    let fallback = translations['en'];
    for (const k of keys) {
      fallback = fallback?.[k];
    }
    return fallback || key;
  }
  return value;
}

export function changeLang(lang) {
  if (translations[lang]) {
    currentLang = lang;
    set(KEYS.LANGUAGE, lang);
    document.documentElement.lang = lang;
    // Disparar evento custom para re-render
    window.dispatchEvent(new CustomEvent('langchange', { detail: { lang } }));
  }
}

export function getCurrentLang() {
  return currentLang;
}

export function getAvailableLangs() {
  return Object.keys(translations);
}
```

### 2.3 `js/theme.js` — Tema Dark/Light

```javascript
// theme.js — Gestión de tema sin React
import { KEYS, get, set } from './storage.js';

let isDark = true;

export function initTheme() {
  const saved = get(KEYS.THEME);
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  
  isDark = saved ? saved === 'dark' : prefersDark;
  applyTheme();
}

export function toggleTheme() {
  isDark = !isDark;
  applyTheme();
  set(KEYS.THEME, isDark ? 'dark' : 'light');
  window.dispatchEvent(new CustomEvent('themechange', { detail: { isDark } }));
}

function applyTheme() {
  document.documentElement.classList.toggle('dark', isDark);
}

export function isDarkMode() {
  return isDark;
}

// Clases temáticas reutilizables
export function getThemeClasses() {
  const dark = isDark;
  return {
    bg: dark ? 'bg-slate-950' : 'bg-slate-50',
    sidebar: dark ? 'bg-slate-900/95 border-slate-800/50 backdrop-blur-sm' : 'bg-white/95 border-slate-200 backdrop-blur-sm',
    card: dark ? 'bg-slate-900/60 border-slate-800/50 hover:bg-slate-800/60' : 'bg-white/90 border-slate-200 hover:bg-slate-50/90 shadow-sm',
    textMain: dark ? 'text-slate-100' : 'text-slate-900',
    textSub: dark ? 'text-slate-400' : 'text-slate-600',
    input: dark ? 'bg-slate-900/80 border-slate-700 text-slate-100 placeholder:text-slate-500' : 'bg-white border-slate-300 text-slate-900 placeholder:text-slate-500',
    header: dark ? 'bg-slate-900/95 border-slate-800/50 backdrop-blur-md' : 'bg-white/95 border-slate-200 backdrop-blur-md',
    aiBox: dark ? 'bg-slate-950/80 border-slate-800/50' : 'bg-slate-50/80 border-slate-200',
  };
}
```

### 2.4 `js/favorites.js` — Gestión de Favoritos

```javascript
// favorites.js — Favoritos sin React
import { KEYS, get, set } from './storage.js';

let favorites = [];

export function initFavorites() {
  favorites = get(KEYS.FAVORITES, []);
}

export function getFavorites() {
  return [...favorites];
}

export function toggleFavorite(tool) {
  const exists = favorites.find(f => f.url === tool.url);
  if (exists) {
    favorites = favorites.filter(f => f.url !== tool.url);
  } else {
    favorites = [...favorites, { name: tool.name, url: tool.url, descKey: tool.descKey, tags: tool.tags }];
  }
  set(KEYS.FAVORITES, favorites);
  window.dispatchEvent(new CustomEvent('favoriteschange', { detail: { favorites } }));
  return !exists; // true si se agregó
}

export function isFavorite(tool) {
  return favorites.some(f => f.url === tool.url);
}

export function exportFavorites() {
  const data = {
    type: 'VeriSource-hub-favorites',
    version: '1.0',
    exportDate: new Date().toISOString(),
    favorites,
    count: favorites.length,
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `VeriSource-hub-favorites-${new Date().toISOString().split('T')[0]}.json`;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
}
```

### 2.5 `js/history.js` — Historial de Búsquedas

```javascript
// history.js — Historial sin React
import { KEYS, get, set, remove } from './storage.js';

let history = [];

export function initHistory() {
  history = get(KEYS.SEARCH_HISTORY, []);
}

export function getHistory() {
  return [...history];
}

let debounceTimer = null;
export function addToHistory(query, results) {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(() => {
    if (query.trim() && query.length > 2) {
      history = [
        { query, results, timestamp: Date.now() },
        ...history.filter(h => h.query !== query),
      ].slice(0, 20);
      set(KEYS.SEARCH_HISTORY, history);
    }
  }, 1000);
}

export function clearHistory() {
  history = [];
  remove(KEYS.SEARCH_HISTORY);
  window.dispatchEvent(new CustomEvent('historychange'));
}
```

### 2.6 `js/search.js` — Búsqueda y Filtrado

```javascript
// search.js — Filtrado sin React useMemo
import { OSINT_DATA } from './data.js';
import { t } from './i18n.js';
import { isFavorite } from './favorites.js';

export function filterTools({ query = '', category = 'all', showFavoritesOnly = false }) {
  let results = OSINT_DATA;

  // Filtrar solo favoritos
  if (showFavoritesOnly) {
    results = results.map(cat => ({
      ...cat,
      items: cat.items.filter(item => isFavorite(item)),
    })).filter(cat => cat.items.length > 0);
  }

  // Filtrar por categoría
  if (!showFavoritesOnly && category !== 'all') {
    results = results.filter(cat => cat.category === category);
  }

  // Filtrar por búsqueda
  if (query) {
    const q = query.toLowerCase();
    results = results.map(cat => ({
      ...cat,
      items: cat.items.filter(item =>
        item.name.toLowerCase().includes(q) ||
        t(`tools.${item.descKey}`).toLowerCase().includes(q) ||
        item.tags.some(tag => tag.toLowerCase().includes(q)) ||
        t(`categories.${cat.category}`).toLowerCase().includes(q)
      ),
    })).filter(cat => cat.items.length > 0);
  }

  return results;
}

export function getCategories() {
  return ['all', ...OSINT_DATA.map(c => c.category)];
}

export function getTotalResults(filtered) {
  return filtered.reduce((acc, cat) => acc + cat.items.length, 0);
}
```

### 2.7 `js/ai.js` — Integración Gemini API

```javascript
// ai.js — Gemini API sin React
import { t, getCurrentLang } from './i18n.js';

const API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent';

export async function callGemini(input, mode, apiKey) {
  if (!apiKey) {
    return { error: true, text: 'Error: API key de Gemini no configurada.' };
  }

  const lang = getCurrentLang();
  let systemPrompt = '';

  if (mode === 'plan') {
    systemPrompt = lang === 'es'
      ? 'Eres un investigador OSINT experto. Basándote en la pista proporcionada, genera un plan de investigación paso a paso. Sugiere categorías de herramientas específicas. Sé conciso y profesional. Responde en español.'
      : 'You are an expert OSINT investigator. Based on the lead provided, generate a step-by-step investigation plan. Suggest specific tool categories. Be concise and professional.';
  } else {
    systemPrompt = lang === 'es'
      ? 'Eres un especialista en Google Dorking. Convierte la solicitud en lenguaje natural del usuario en una cadena de Google Dork válida. Proporciona el Dork claramente, seguido de una breve explicación. Responde en español.'
      : 'You are a Google Dorking specialist. Convert the user\'s natural language request into a valid Google Dork string. Provide the Dork itself clearly, followed by a brief explanation.';
  }

  const payload = {
    contents: [{ parts: [{ text: input }] }],
    systemInstruction: { parts: [{ text: systemPrompt }] },
  };

  let retries = 0;
  const maxRetries = 3;

  while (retries <= maxRetries) {
    try {
      const response = await fetch(`${API_URL}?key=${apiKey}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload),
      });

      if (!response.ok) throw new Error(`HTTP ${response.status}`);

      const result = await response.json();
      const text = result.candidates?.[0]?.content?.parts?.[0]?.text;
      return { error: false, text: text || 'No se recibió respuesta.' };
    } catch (err) {
      retries++;
      if (retries > maxRetries) {
        return { error: true, text: t('ai.error') };
      }
      await new Promise(r => setTimeout(r, Math.pow(2, retries) * 500));
    }
  }
}
```

### 2.8 `js/keyboard.js` — Atajos de Teclado

```javascript
// keyboard.js — Atajos globales sin React
export function initKeyboardShortcuts(handlers) {
  window.addEventListener('keydown', (e) => {
    // No interceptar en inputs
    if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') {
      // Solo Escape funciona en inputs
      if (e.key === 'Escape' && handlers.escape) {
        handlers.escape();
      }
      return;
    }

    // Ctrl/Cmd + K → Focus búsqueda
    if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
      e.preventDefault();
      handlers.focusSearch?.();
    }

    // Escape → Cerrar modales
    if (e.key === 'Escape') {
      handlers.escape?.();
    }

    // Ctrl/Cmd + Shift + F → Favoritos
    if ((e.ctrlKey || e.metaKey) && e.shiftKey && e.key === 'F') {
      e.preventDefault();
      handlers.toggleFavorites?.();
    }

    // Ctrl/Cmd + Shift + H → Historial
    if ((e.ctrlKey || e.metaKey) && e.shiftKey && e.key === 'H') {
      e.preventDefault();
      handlers.toggleHistory?.();
    }

    // Ctrl/Cmd + Shift + D → Tema
    if ((e.ctrlKey || e.metaKey) && e.shiftKey && e.key === 'D') {
      e.preventDefault();
      handlers.toggleTheme?.();
    }
  });
}
```

### 2.9 `js/render.js` — Funciones de Renderizado

```javascript
// render.js — Renderizado DOM con template literals
import { t } from './i18n.js';
import { isDarkMode, getThemeClasses } from './theme.js';
import { isFavorite, toggleFavorite } from './favorites.js';
import { filterTools, getTotalResults } from './search.js';
import { getTagColor } from './tags.js';
import { getIconSVG } from './icons.js';

// Estado de la vista
let currentView = 'main'; // 'main' | 'favorites' | 'history' | 'ai'
let currentCategory = 'all';
let currentQuery = '';

export function setView(view) {
  currentView = view;
  render();
}

export function setCategory(cat) {
  currentCategory = cat;
  render();
}

export function setQuery(query) {
  currentQuery = query;
  render();
}

export function render() {
  const theme = getThemeClasses();
  const container = document.getElementById('main-content');

  switch (currentView) {
    case 'favorites':
      container.innerHTML = renderFavoritesView(theme);
      break;
    case 'history':
      container.innerHTML = renderHistoryView(theme);
      break;
    case 'ai':
      container.innerHTML = renderAIView(theme);
      break;
    default:
      container.innerHTML = renderMainView(theme);
  }

  // Re-bindear eventos después de cada render
  bindEvents();
  updateResultsCount();
}

function renderMainView(theme) {
  const filtered = filterTools({
    query: currentQuery,
    category: currentCategory,
  });

  if (filtered.length === 0) {
    return renderEmptyState(theme);
  }

  return `
    <div class="animate-fade-in">
      ${renderSearchBar(theme)}
      <div class="max-w-6xl mx-auto space-y-12">
        ${filtered.map((cat, i) => renderCategory(cat, i, theme)).join('')}
      </div>
    </div>
  `;
}

function renderCategory(category, index, theme) {
  return `
    <section class="animate-slide-in-from-bottom" style="animation-delay: ${index * 100}ms">
      <div class="flex items-center gap-4 mb-8">
        <div class="relative p-3 rounded-xl bg-gradient-to-r ${category.color} shadow-lg">
          ${getIconSVG(category.icon)}
        </div>
        <div>
          <h2 class="text-2xl font-bold tracking-tight ${isDarkMode() ? 'text-white' : 'text-slate-900'}">
            ${t('categories.' + category.category)}
          </h2>
          <p class="text-sm ${theme.textSub} mt-1">
            ${category.items.length} ${category.items.length === 1 ? 'herramienta' : 'herramientas'}
          </p>
        </div>
        <div class="h-px flex-1 ml-6 hidden md:block bg-gradient-to-r ${category.color} opacity-30"></div>
      </div>
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6">
        ${category.items.map((item, idx) => renderToolCard(item, category, idx, index, theme)).join('')}
      </div>
    </section>
  `;
}

function renderToolCard(item, category, idx, catIdx, theme) {
  const fav = isFavorite(item);
  return `
    <div class="group relative p-6 rounded-2xl border transition-all duration-300 card-hover backdrop-blur-sm
      ${theme.card} hover:border-VeriSource-500/40 hover:shadow-xl hover:shadow-VeriSource-500/5"
      style="animation-delay: ${(catIdx * 100) + (idx * 50)}ms">
      <div class="flex justify-between items-start mb-4">
        <div class="flex-1 min-w-0">
          <h3 class="font-semibold transition-colors truncate pr-4 text-lg
            ${isDarkMode() ? 'text-slate-100 group-hover:text-white' : 'text-slate-800 group-hover:text-VeriSource-600'}">
            ${item.name}
          </h3>
          <div class="flex items-center gap-2 mt-2">
            ${item.tags.slice(0, 2).map(tag =>
              `<span class="px-2 py-1 rounded-md text-xs font-medium transition-colors ${getTagColor(tag)}">${tag}</span>`
            ).join('')}
          </div>
        </div>
        <div class="flex items-center gap-2 shrink-0">
          <button data-action="toggle-fav" data-url="${item.url}" data-name="${item.name}" data-desc="${item.descKey}" data-tags='${JSON.stringify(item.tags)}'
            class="p-2 rounded-lg transition-all hover:scale-110 ${fav ? 'text-yellow-500 hover:text-yellow-400' : 'text-slate-400 hover:text-yellow-500'}">
            ${fav ? '★' : '☆'}
          </button>
          <a href="${item.url}" target="_blank" rel="noopener noreferrer"
            class="p-2 rounded-lg transition-all hover:scale-110 ${isDarkMode() ? 'text-slate-600 hover:text-VeriSource-400' : 'text-slate-300 hover:text-VeriSource-600'}">
            ${getIconSVG('external-link')}
          </a>
        </div>
      </div>
      <p class="text-sm line-clamp-2 leading-relaxed ${theme.textSub}">
        ${t('tools.' + item.descKey)}
      </p>
      <div class="absolute inset-0 rounded-2xl bg-gradient-to-br from-VeriSource-500/5 to-transparent opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"></div>
    </div>
  `;
}

function bindEvents() {
  // Bind favorite toggles
  document.querySelectorAll('[data-action="toggle-fav"]').forEach(btn => {
    btn.addEventListener('click', (e) => {
      e.preventDefault();
      const tool = {
        url: btn.dataset.url,
        name: btn.dataset.name,
        descKey: btn.dataset.desc,
        tags: JSON.parse(btn.dataset.tags),
      };
      toggleFavorite(tool);
      render(); // Re-render
    });
  });
}
```

### 2.10 `js/app.js` — Orquestador Principal

```javascript
// app.js — Inicialización y orquestación
import { initTheme, toggleTheme } from './theme.js';
import { initI18n, changeLang } from './i18n.js';
import { initFavorites } from './favorites.js';
import { initHistory } from './history.js';
import { initKeyboardShortcuts } from './keyboard.js';
import { render, setView, setCategory, setQuery } from './render.js';

// Inicialización
document.addEventListener('DOMContentLoaded', () => {
  // Módulos
  initTheme();
  initI18n();
  initFavorites();
  initHistory();

  // Render inicial
  render();

  // Atajos de teclado
  initKeyboardShortcuts({
    focusSearch: () => document.getElementById('search-input')?.focus(),
    escape: () => {
      setView('main');
      document.getElementById('sidebar')?.classList.add('-translate-x-full');
    },
    toggleFavorites: () => setView('favorites'),
    toggleHistory: () => setView('history'),
    toggleTheme,
  });

  // Event listeners globales
  document.getElementById('search-input')?.addEventListener('input', (e) => {
    setQuery(e.target.value);
  });

  document.getElementById('theme-toggle')?.addEventListener('click', toggleTheme);

  // Escuchar cambios para re-render
  window.addEventListener('themechange', render);
  window.addEventListener('langchange', render);
  window.addEventListener('favoriteschange', render);
  window.addEventListener('historychange', render);
});
```

## Fase 3: HTML Principal

### 3.1 `index.html` — Esqueleto

```html
<!DOCTYPE html>
<html lang="es" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Hub completo de recursos VeriSource con inteligencia artificial integrada">
  <link rel="icon" type="image/svg+xml" href="/public/favicon.svg">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Fira+Code:wght@300;400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="./css/output.css">
  <title>VeriSource - Recursos de Inteligencia Digital</title>

  <!-- Theme initialization ANTES del CSS para evitar FOUC -->
  <script>
    (function() {
      const saved = localStorage.getItem('osint-hub-theme');
      const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
      const isDark = saved ? saved === 'dark' : prefersDark;
      document.documentElement.classList.toggle('dark', isDark);
    })();
  </script>
</head>
<body class="min-h-screen font-sans transition-all duration-500">

  <!-- Header -->
  <header id="header" class="flex items-center justify-between p-3 sm:p-4 sticky top-0 z-50 border-b lg:px-8">
    <div class="flex items-center gap-3">
      <!-- Logo + Título -->
      <div class="relative">
        <svg class="w-6 sm:w-7 h-6 sm:h-7 text-VeriSource-500 animate-pulse"><!-- Shield icon --></svg>
      </div>
      <div>
        <h1 class="font-bold text-lg sm:text-xl tracking-tight">
          VeriSource <span class="text-VeriSource-500 font-display">HUB</span>
        </h1>
        <p class="text-xs hidden sm:block" data-i18n="header.subtitle"></p>
      </div>
    </div>
    <div class="flex items-center gap-2 lg:gap-4">
      <!-- Language selector, theme toggle, mobile menu -->
      <div id="lang-selector" class="relative group hidden sm:block">
        <button class="p-2 sm:p-2.5 rounded-xl transition-all">
          <svg class="w-4 sm:w-5 h-4 sm:h-5"><!-- Languages icon --></svg>
        </button>
        <div id="lang-dropdown" class="absolute right-0 top-full mt-2 w-32 py-2 rounded-xl border shadow-xl opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all">
          <!-- Populated by JS -->
        </div>
      </div>
      <button id="theme-toggle" class="p-2 sm:p-2.5 rounded-xl transition-all hover:scale-105">
        <!-- Sun/Moon icon -->
      </button>
      <button id="sidebar-toggle" class="lg:hidden p-2 sm:p-2.5 rounded-xl transition-all hover:scale-105">
        <!-- Menu/X icon -->
      </button>
    </div>
  </header>

  <div class="flex">
    <!-- Sidebar -->
    <aside id="sidebar" class="fixed lg:sticky top-0 lg:top-[73px] left-0 z-40 h-[calc(100vh-73px)] w-full sm:w-80 lg:w-72 border-r transition-all duration-300 -translate-x-full lg:translate-x-0">
      <nav class="px-4 py-6 overflow-y-auto h-full custom-scrollbar">
        <div id="categories-list" class="mb-6">
          <!-- Populated by JS -->
        </div>
        <div id="results-count" class="mt-6 p-4 rounded-xl border">
          <!-- Stats -->
        </div>
      </nav>
    </aside>

    <!-- Main Content -->
    <main id="main-content" class="flex-1 min-w-0 p-3 sm:p-6 lg:p-8">
      <!-- Populated by render.js -->
    </main>
  </div>

  <!-- Scripts como ES Modules -->
  <script type="module" src="./js/app.js"></script>
</body>
</html>
```

## Fase 4: CSS Custom

### 4.1 `css/input.css`

Copiar exactamente el contenido de `src/index.css` actual. Solo cambiar la importación de fuentes si se usa el link en HTML.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Copiar todo el CSS custom de src/index.css */
/* glass, gradient-text, card-hover, animations, scrollbar, etc. */
```

## Fase 5: Iconos

### Opción A: Lucide Vanilla (Recomendado)

```bash
npm install lucide
```

```javascript
// icons.js
import { createIcons, Search, ExternalLink, Shield, Globe, User, Map, /* ... */ } from 'lucide';

export function initIcons() {
  createIcons();
}

// Llamar después de cada render
export function refreshIcons() {
  createIcons();
}
```

En HTML usar: `<i data-lucide="search"></i>`

### Opción B: SVGs Inline (Zero-dependency)

Crear un objeto con los SVGs:

```javascript
const ICONS = {
  search: '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>',
  // ... cada icono usado
};

export function getIconSVG(name, size = 20) {
  return ICONS[name] || '';
}
```

## Fase 6: Safelist de Tailwind

Las clases dinámicas generadas con template literals necesitan estar en safelist:

```javascript
// En tailwind.config.js
safelist: [
  // Gradientes de categorías
  'from-blue-500', 'to-cyan-500',
  'from-pink-500', 'to-rose-500',
  'from-purple-500', 'to-indigo-500',
  'from-emerald-500', 'to-teal-500',
  'from-orange-500', 'to-red-500',
  'from-yellow-500', 'to-amber-500',
  'from-violet-500', 'to-purple-500',
  'from-green-500', 'to-emerald-500',
  // Tag colors (generadas dinámicamente)
  { pattern: /bg-(blue|pink|purple|emerald|orange|yellow|violet|green|red|cyan|sky|amber|slate|indigo|gray)-(500|600)\/20/ },
  { pattern: /text-(blue|pink|purple|emerald|orange|yellow|violet|green|red|cyan|sky|amber|slate|indigo|gray)-(300|400)/ },
  { pattern: /border-(blue|pink|purple|emerald|orange|yellow|violet|green|red|cyan|sky|amber|slate|indigo|gray)-(500|600)\/30/ },
]
```

## Fase 7: Deploy

### Vercel (estático)

Crear `vercel.json`:

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": ".",
  "framework": null,
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

### GitHub Pages / Netlify

Solo servir archivos estáticos directamente. No necesita build excepto Tailwind CSS.

## Checklist de Migración

- [ ] Crear estructura de archivos nueva
- [ ] Migrar `storage.js`
- [ ] Migrar `i18n.js` + datos de traducción
- [ ] Migrar `theme.js`
- [ ] Migrar `data.js` (eliminar JSX/Lucide React)
- [ ] Migrar `favorites.js`
- [ ] Migrar `history.js`
- [ ] Migrar `search.js`
- [ ] Migrar `ai.js`
- [ ] Migrar `keyboard.js`
- [ ] Crear `render.js` con todas las vistas
- [ ] Crear `icons.js` (Lucide vanilla o SVGs inline)
- [ ] Crear `app.js` orquestador
- [ ] Crear `index.html` con markup completo
- [ ] Copiar y adaptar CSS
- [ ] Adaptar `tailwind.config.js` con safelist
- [ ] Configurar build scripts
- [ ] Probar todas las funcionalidades
- [ ] Eliminar dependencias React del package.json
- [ ] Deploy a Vercel/static
