# Traducciones Completas ES/EN — Referencia

Este documento contiene todas las traducciones del sistema i18n que deben migrarse de React Context a JavaScript vanilla.

## Estructura de Traducción

Las traducciones usan dot-notation accedida via `t('section.key')`:

```
translations
├── es (Español - idioma por defecto)
│   ├── nav.*
│   ├── header.*
│   ├── search.*
│   ├── categories.*
│   ├── ai.*
│   ├── tools.*        ← Descripciones de CADA herramienta
│   └── common.*
└── en (English)
    ├── nav.*
    ├── header.*
    ├── search.*
    ├── categories.*
    ├── ai.*
    ├── tools.*
    └── common.*
```

## Datos Completos de Traducción

```javascript
// translations-data.js — Traducciones sin dependencias React
export const translations = {
  es: {
    // Navegación
    nav: {
      home: "Inicio",
      categories: "Categorías",
      favorites: "Favoritos",
      settings: "Configuración",
      about: "Acerca de"
    },

    // Header
    header: {
      title: "VeriSource",
      subtitle: "Recursos de Inteligencia Digital",
      darkMode: "Cambiar a modo oscuro",
      lightMode: "Cambiar a modo claro",
      language: "Cambiar idioma",
      menu: "Menú"
    },

    // Búsqueda
    search: {
      placeholder: "Buscar herramientas, plataformas o palabras clave...",
      results: "resultados",
      noResults: "Sin resultados",
      noResultsDesc: "Intenta ajustar tu búsqueda o filtros",
      advanced: "Búsqueda avanzada"
    },

    // Categorías
    categories: {
      all: "Todo",
      searchEngines: "Motores de Búsqueda",
      socialMedia: "Redes Sociales",
      personSearch: "Búsqueda de Personas",
      imageMapping: "Imágenes y Mapas",
      corporateTech: "Corporativo y Tecnología",
      transportLive: "Transporte y Tiempo Real",
      artificialIntelligence: "Inteligencia Artificial",
      verificationBlogs: "Verificación y Blogs"
    },

    // Asistente IA
    ai: {
      title: "VeriSource AI Suite",
      subtitle: "Planifica investigaciones y genera dorks automáticamente con Gemini.",
      investigatorPlan: "Plan de Investigación",
      dorkGenerator: "Generador de Dorks",
      leadAnalysis: "Análisis de Pistas",
      querySynthesis: "Síntesis de Consultas",
      planPlaceholder: "Entrada: @usuario, email, dominio...",
      dorkPlaceholder: "¿Qué buscas? (ej: archivos pdf de gobierno)...",
      generatePlan: "Generar Plan",
      synthesizeDork: "Sintetizar Dork",
      results: "Resultados",
      copy: "Copiar",
      loading: "Procesando...",
      error: "Error: No se pudo conectar con la API de Gemini después de múltiples intentos."
    },

    // Descripciones de herramientas (cada descKey)
    tools: {
      // Motores de Búsqueda
      google: "Motor de búsqueda general",
      googleAdvanced: "Parámetros de búsqueda refinados",
      googleHacking: "Google Dorks para vulnerabilidades",
      bing: "Motor de búsqueda de Microsoft",
      yandex: "Motor de búsqueda ruso",
      baidu: "Motor de búsqueda chino",
      duckduckgo: "Búsqueda centrada en privacidad",
      startpage: "Resultados de Google sin rastreo",

      // Redes Sociales
      twitonomy: "Análisis de Twitter",
      botSentinel: "Rastrear actividad de bots",
      waybackTweets: "Tweets archivados",
      instagramExplorer: "Herramienta IG de VeriSource Combine",
      picuki: "Editor y visor de Instagram",
      tiktokSearch: "Investigaciones de TikTok",
      telegago: "Búsqueda de Telegram",
      graphTips: "Búsqueda filtrada de Facebook",

      // Búsqueda de Personas
      emailRep: "Validar direcciones de email",
      epieos: "Identificar cuentas de Gmail",
      hunter: "Buscador de direcciones de email",
      instantUsername: "Buscar usuarios en plataformas",
      whatsmyname: "Búsqueda exhaustiva de nombres de usuario",
      haveibeenpwned: "Verificar filtraciones de datos",
      pipl: "Búsqueda global de personas",
      thatsthem: "Herramientas de búsqueda inversa",
      idcrawl: "Búsqueda inversa de personas",

      // Imágenes y Mapas
      tineye: "Búsqueda inversa de imágenes",
      facecheck: "Búsqueda por reconocimiento facial",
      suncalc: "Calculadora de posición solar y sombras",
      fotoforensics: "Metadatos de imagen y ELA",
      dualmaps: "Vista de calle lado a lado",
      zoomearth: "Imágenes satelitales y clima",
      peakfinder: "Identificar picos montañosos",

      // Corporativo y Tecnología
      opencorporates: "Base de datos global de empresas",
      offshoreleaks: "Investigar filtraciones financieras",
      builtwith: "Perfil tecnológico de sitios web",
      urlscan: "Analizar comportamiento de sitios web",
      dnsdumpster: "Investigación de dominios",
      cyberchef: "La navaja suiza cibernética",

      // Transporte y Tiempo Real
      marinetraffic: "Seguimiento global de barcos",
      radarbox: "Rastreador de vuelos en vivo",
      openrailway: "Infraestructura ferroviaria",
      broadcastify: "Transmisiones de radio en vivo",
      windy: "Clima en vivo y webcams",

      // Inteligencia Artificial
      chatgpt: "Modelo de lenguaje OpenAI",
      gemini: "IA de Google",
      claude: "IA de Anthropic",
      consensus: "IA para papers de investigación",
      undetectable: "Detector y humanizador de IA",

      // Verificación y Blogs
      bellingcat: "Periodismo de investigación",
      snopes: "Recurso de verificación de hechos",
      VeriSourceframework: "Directorio completo de herramientas",
      inteltechniques: "Recursos de Michael Bazzell",
      sector035: "Blog Semana en VeriSource"
    },

    // Textos comunes
    common: {
      loading: "Cargando...",
      error: "Error",
      success: "Éxito",
      cancel: "Cancelar",
      confirm: "Confirmar",
      save: "Guardar",
      delete: "Eliminar",
      edit: "Editar",
      view: "Ver",
      close: "Cerrar",
      back: "Volver",
      next: "Siguiente",
      previous: "Anterior"
    }
  },

  en: {
    // Navigation
    nav: {
      home: "Home",
      categories: "Categories",
      favorites: "Favorites",
      settings: "Settings",
      about: "About"
    },

    // Header
    header: {
      title: "VeriSource",
      subtitle: "Digital Intelligence Resources",
      darkMode: "Switch to dark mode",
      lightMode: "Switch to light mode",
      language: "Change language",
      menu: "Menu"
    },

    // Search
    search: {
      placeholder: "Search tools, platforms or keywords...",
      results: "results",
      noResults: "No results found",
      noResultsDesc: "Try adjusting your search or filters",
      advanced: "Advanced search"
    },

    // Categories
    categories: {
      all: "All",
      searchEngines: "Search Engines",
      socialMedia: "Social Media",
      personSearch: "Person Search",
      imageMapping: "Image & Mapping",
      corporateTech: "Corporate & Tech",
      transportLive: "Transport & Live",
      artificialIntelligence: "Artificial Intelligence",
      verificationBlogs: "Verification & Blogs"
    },

    // AI Assistant
    ai: {
      title: "VeriSource AI Suite",
      subtitle: "Plan investigations and generate dorks automatically with Gemini.",
      investigatorPlan: "Investigator Plan",
      dorkGenerator: "Dork Generator",
      leadAnalysis: "Lead Analysis",
      querySynthesis: "Query Synthesis",
      planPlaceholder: "Input: @username, email, domain...",
      dorkPlaceholder: "What are you looking for? (eg: government pdf files)...",
      generatePlan: "Generate Plan",
      synthesizeDork: "Synthesize Dork",
      results: "Results",
      copy: "Copy",
      loading: "Processing...",
      error: "Error: Failed to connect to Gemini API after multiple attempts."
    },

    // Tools descriptions
    tools: {
      // Search Engines
      google: "General search engine",
      googleAdvanced: "Refined search parameters",
      googleHacking: "Google Dorks for vulnerabilities",
      bing: "Microsoft search engine",
      yandex: "Russian search engine",
      baidu: "Chinese search engine",
      duckduckgo: "Privacy-focused search",
      startpage: "Google results without tracking",

      // Social Media
      twitonomy: "Twitter analytics",
      botSentinel: "Track bot activity",
      waybackTweets: "Archived tweets",
      instagramExplorer: "VeriSource Combine IG tool",
      picuki: "Instagram editor & viewer",
      tiktokSearch: "TikTok investigations",
      telegago: "Telegram search",
      graphTips: "Facebook filter search",

      // Person Search
      emailRep: "Validate email addresses",
      epieos: "Identify Gmail accounts",
      hunter: "Email address finder",
      instantUsername: "Search handles across platforms",
      whatsmyname: "Extensive username search",
      haveibeenpwned: "Check data breaches",
      pipl: "Global people search",
      thatsthem: "Reverse lookup tools",
      idcrawl: "Reverse people search",

      // Image & Mapping
      tineye: "Reverse image search",
      facecheck: "Facial recognition search",
      suncalc: "Shadow and sun position calculator",
      fotoforensics: "Image metadata & ELA",
      dualmaps: "Street view side-by-side",
      zoomearth: "Satellite imagery & weather",
      peakfinder: "Identify mountain peaks",

      // Corporate & Tech
      opencorporates: "Global company database",
      offshoreleaks: "Investigate financial leaks",
      builtwith: "Website technology profile",
      urlscan: "Analyze website behavior",
      dnsdumpster: "Domain research",
      cyberchef: "The Cyber Swiss Army Knife",

      // Transport & Live
      marinetraffic: "Global ship tracking",
      radarbox: "Live flight tracker",
      openrailway: "Railway infrastructure",
      broadcastify: "Live radio scanner feeds",
      windy: "Live weather & webcams",

      // AI
      chatgpt: "OpenAI language model",
      gemini: "Google AI",
      claude: "Anthropic AI",
      consensus: "AI for research papers",
      undetectable: "AI detector & humanizer",

      // Verification & Blogs
      bellingcat: "Investigative journalism",
      snopes: "Fact-checking resource",
      VeriSourceframework: "Comprehensive tool directory",
      inteltechniques: "Michael Bazzell's resources",
      sector035: "Week in VeriSource blog"
    },

    // Common
    common: {
      loading: "Loading...",
      error: "Error",
      success: "Success",
      cancel: "Cancel",
      confirm: "Confirm",
      save: "Save",
      delete: "Delete",
      edit: "Edit",
      view: "View",
      close: "Close",
      back: "Back",
      next: "Next",
      previous: "Previous"
    }
  }
};
```

## Implementación Vanilla del Sistema i18n

```javascript
// i18n.js — Sistema de traducción sin React
import { translations } from './translations-data.js';

let currentLang = 'es';

/**
 * Inicializar idioma: localStorage > navegador > fallback 'es'
 */
export function initI18n() {
  const stored = localStorage.getItem('VeriSource-hub-language');
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

/**
 * Traducir clave con dot-notation.
 * Fallback: inglés → clave literal
 * 
 * @example t('categories.searchEngines') → "Motores de Búsqueda"
 * @example t('tools.google') → "Motor de búsqueda general"
 */
export function t(key) {
  const keys = key.split('.');
  let value = translations[currentLang];

  for (const k of keys) {
    value = value?.[k];
  }

  // Fallback a inglés
  if (value === undefined || value === null) {
    let fallback = translations['en'];
    for (const k of keys) {
      fallback = fallback?.[k];
    }
    return fallback ?? key;
  }

  return value;
}

/**
 * Cambiar idioma y persistir
 */
export function changeLang(lang) {
  if (translations[lang]) {
    currentLang = lang;
    localStorage.setItem('VeriSource-hub-language', lang);
    document.documentElement.lang = lang;
    
    // Notificar cambio para re-render
    window.dispatchEvent(new CustomEvent('langchange', { detail: { lang } }));
  }
}

/**
 * Obtener idioma actual
 */
export function getCurrentLang() {
  return currentLang;
}

/**
 * Obtener idiomas disponibles
 */
export function getAvailableLangs() {
  return Object.keys(translations);
}
```

## Uso en HTML con `data-i18n`

Para textos estáticos del HTML, usar atributos `data-i18n`:

```html
<p data-i18n="header.subtitle"></p>
<input data-i18n-placeholder="search.placeholder">
<button data-i18n="ai.generatePlan"></button>
```

Función de actualización masiva:

```javascript
export function updateI18nElements() {
  // Texto
  document.querySelectorAll('[data-i18n]').forEach(el => {
    el.textContent = t(el.dataset.i18n);
  });

  // Placeholder
  document.querySelectorAll('[data-i18n-placeholder]').forEach(el => {
    el.placeholder = t(el.dataset.i18nPlaceholder);
  });

  // Title/aria-label
  document.querySelectorAll('[data-i18n-title]').forEach(el => {
    el.title = t(el.dataset.i18nTitle);
  });
}
```

## Claves de Traducción — Índice Rápido

| Sección | Claves | Ejemplo |
|---------|--------|---------|
| `nav.*` | 5 | `nav.home`, `nav.categories` |
| `header.*` | 6 | `header.title`, `header.subtitle` |
| `search.*` | 5 | `search.placeholder`, `search.noResults` |
| `categories.*` | 9 | `categories.all`, `categories.searchEngines` |
| `ai.*` | 13 | `ai.title`, `ai.generatePlan`, `ai.error` |
| `tools.*` | 42 | `tools.google`, `tools.bellingcat` |
| `common.*` | 13 | `common.loading`, `common.close` |
| **Total** | **93** | — |
