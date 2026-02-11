# Catálogo de Datos OSINT — Referencia Completa

Este documento contiene la estructura de datos completa de herramientas OSINT que debe migrarse de JSX a JavaScript puro.

## Estructura de Cada Categoría

```javascript
{
  category: "string",      // Clave única (usada en i18n: categories.{category})
  icon: "string",          // Nombre del icono Lucide (en vez de JSX <Component />)
  color: "string",         // Clases Tailwind gradient: "from-X to-Y"
  items: [
    {
      name: "string",      // Nombre para mostrar
      url: "string",       // URL de la herramienta
      descKey: "string",   // Clave i18n: tools.{descKey}
      tags: ["string"]     // Tags para búsqueda y color-coding
    }
  ]
}
```

## Datos Completos (JavaScript Puro — sin JSX)

```javascript
// data.js — Catálogo OSINT como objeto JavaScript puro
export const OSINT_DATA = [
  {
    category: "searchEngines",
    icon: "search",
    color: "from-blue-500 to-cyan-500",
    items: [
      { name: "Google", url: "https://www.google.com/", descKey: "google", tags: ["general", "popular", "worldwide"] },
      { name: "Google Advanced Search", url: "https://www.google.com/advanced_search", descKey: "googleAdvanced", tags: ["advanced", "filters", "precise"] },
      { name: "Google Hacking Database", url: "https://www.exploit-db.com/google-hacking-database", descKey: "googleHacking", tags: ["security", "dorks", "vulnerabilities"] },
      { name: "Bing", url: "https://www.bing.com/", descKey: "bing", tags: ["microsoft", "alternative", "images"] },
      { name: "Yandex", url: "https://yandex.com/", descKey: "yandex", tags: ["russia", "cyrillic", "regional"] },
      { name: "Baidu", url: "https://www.baidu.com/", descKey: "baidu", tags: ["china", "chinese", "regional"] },
      { name: "DuckDuckGo", url: "https://duckduckgo.com/", descKey: "duckduckgo", tags: ["privacy", "anonymous", "no-tracking"] },
      { name: "Startpage", url: "https://www.startpage.com/", descKey: "startpage", tags: ["privacy", "google-proxy", "anonymous"] }
    ]
  },
  {
    category: "socialMedia",
    icon: "message-square",
    color: "from-pink-500 to-rose-500",
    items: [
      { name: "Twitonomy", url: "https://www.twitonomy.com/", descKey: "twitonomy", tags: ["twitter", "analytics", "statistics"] },
      { name: "Bot Sentinel", url: "https://botsentinel.com/", descKey: "botSentinel", tags: ["bots", "automation", "detection"] },
      { name: "Wayback Tweets", url: "https://waybacktweets.streamlit.app/", descKey: "waybackTweets", tags: ["archive", "deleted", "history"] },
      { name: "Instagram Explorer", url: "https://www.osintcombine.com/free-osint-tools/instagram-explorer", descKey: "instagramExplorer", tags: ["instagram", "profiles", "analysis"] },
      { name: "Picuki", url: "https://www.picuki.com/", descKey: "picuki", tags: ["instagram", "viewer", "anonymous"] },
      { name: "TikTok Quick Search", url: "https://www.osintcombine.com/free-osint-tools/tiktok-quick-search", descKey: "tiktokSearch", tags: ["tiktok", "videos", "search"] },
      { name: "Telegago", url: "https://cse.google.com/cse?&cx=006368593537057042503:efxu7xprihg", descKey: "telegago", tags: ["telegram", "channels", "search"] },
      { name: "Graph.tips", url: "http://graph.tips/beta/", descKey: "graphTips", tags: ["facebook", "search", "filters"] }
    ]
  },
  {
    category: "personSearch",
    icon: "user",
    color: "from-purple-500 to-indigo-500",
    items: [
      { name: "Email Reputation", url: "https://emailrep.io/", descKey: "emailRep", tags: ["email", "validation", "reputation"] },
      { name: "Epieos Tools", url: "https://tools.epieos.com/google-account.php", descKey: "epieos", tags: ["gmail", "google", "accounts"] },
      { name: "Hunter.io", url: "https://hunter.io/", descKey: "hunter", tags: ["email", "finder", "business"] },
      { name: "Instant Username", url: "https://instantusername.com/", descKey: "instantUsername", tags: ["username", "social", "availability"] },
      { name: "WhatsMyName", url: "https://whatsmyname.app/", descKey: "whatsmyname", tags: ["username", "comprehensive", "platforms"] },
      { name: "Have I Been Pwned", url: "https://haveibeenpwned.com/", descKey: "haveibeenpwned", tags: ["breach", "security", "passwords"] },
      { name: "Pipl", url: "https://pipl.com/", descKey: "pipl", tags: ["people", "global", "deep-web"] },
      { name: "ThatsThem", url: "https://thatsthem.com/people-search", descKey: "thatsthem", tags: ["reverse", "phone", "address"] },
      { name: "IDCrawl", url: "https://www.idcrawl.com/", descKey: "idcrawl", tags: ["people", "reverse", "search"] }
    ]
  },
  {
    category: "imageMapping",
    icon: "map",
    color: "from-emerald-500 to-teal-500",
    items: [
      { name: "Tineye", url: "https://tineye.com/", descKey: "tineye", tags: ["reverse", "image", "search"] },
      { name: "FaceCheck.id", url: "https://facecheck.id/", descKey: "facecheck", tags: ["face", "recognition", "ai"] },
      { name: "Suncalc", url: "https://www.suncalc.org/", descKey: "suncalc", tags: ["geolocation", "shadow", "time"] },
      { name: "FotoForensics", url: "https://fotoforensics.com/", descKey: "fotoforensics", tags: ["metadata", "forensics", "analysis"] },
      { name: "Dual Maps", url: "https://www.mapchannels.com/DualMaps.aspx", descKey: "dualmaps", tags: ["streetview", "comparison", "maps"] },
      { name: "Zoom Earth", url: "https://zoom.earth/", descKey: "zoomearth", tags: ["satellite", "weather", "live"] },
      { name: "PeakFinder", url: "https://www.peakfinder.org/", descKey: "peakfinder", tags: ["mountains", "geography", "identification"] }
    ]
  },
  {
    category: "corporateTech",
    icon: "briefcase",
    color: "from-orange-500 to-red-500",
    items: [
      { name: "OpenCorporates", url: "https://opencorporates.com/", descKey: "opencorporates", tags: ["companies", "corporate", "database"] },
      { name: "ICIJ Offshore Leaks", url: "http://offshoreleaks.icij.org/", descKey: "offshoreleaks", tags: ["panama-papers", "offshore", "financial"] },
      { name: "BuiltWith", url: "https://builtwith.com/", descKey: "builtwith", tags: ["technology", "stack", "website"] },
      { name: "Urlscan.io", url: "https://urlscan.io/", descKey: "urlscan", tags: ["url", "analysis", "security"] },
      { name: "DNS Dumpster", url: "https://dnsdumpster.com/", descKey: "dnsdumpster", tags: ["dns", "domain", "infrastructure"] },
      { name: "CyberChef", url: "https://gchq.github.io/CyberChef/", descKey: "cyberchef", tags: ["encoding", "decoding", "analysis"] }
    ]
  },
  {
    category: "transportLive",
    icon: "activity",
    color: "from-yellow-500 to-amber-500",
    items: [
      { name: "MarineTraffic", url: "https://www.marinetraffic.com/", descKey: "marinetraffic", tags: ["ships", "maritime", "tracking"] },
      { name: "RadarBox", url: "https://www.radarbox24.com/", descKey: "radarbox", tags: ["flights", "aircraft", "live"] },
      { name: "OpenRailwayMap", url: "https://www.openrailwaymap.org/", descKey: "openrailway", tags: ["railway", "trains", "infrastructure"] },
      { name: "Broadcastify", url: "https://www.broadcastify.com/listen/", descKey: "broadcastify", tags: ["radio", "scanner", "emergency"] },
      { name: "Windy", url: "https://www.windy.com/", descKey: "windy", tags: ["weather", "webcams", "live"] }
    ]
  },
  {
    category: "artificialIntelligence",
    icon: "cpu",
    color: "from-violet-500 to-purple-500",
    items: [
      { name: "ChatGPT", url: "https://chatgpt.com/", descKey: "chatgpt", tags: ["openai", "language", "assistant"] },
      { name: "Gemini", url: "https://gemini.google.com/", descKey: "gemini", tags: ["google", "multimodal", "ai"] },
      { name: "Claude", url: "https://claude.ai/", descKey: "claude", tags: ["anthropic", "helpful", "harmless"] },
      { name: "Consensus", url: "https://consensus.app/", descKey: "consensus", tags: ["research", "papers", "scientific"] },
      { name: "Undetectable AI", url: "https://undetectable.ai/", descKey: "undetectable", tags: ["detection", "humanizer", "bypass"] }
    ]
  },
  {
    category: "verificationBlogs",
    icon: "check-circle",
    color: "from-green-500 to-emerald-500",
    items: [
      { name: "Bellingcat", url: "https://www.bellingcat.com/", descKey: "bellingcat", tags: ["journalism", "investigation", "verification"] },
      { name: "Snopes", url: "https://www.snopes.com/", descKey: "snopes", tags: ["fact-check", "debunking", "rumors"] },
      { name: "OSINT Framework", url: "https://osintframework.com/", descKey: "osintframework", tags: ["directory", "comprehensive", "tools"] },
      { name: "IntelTechniques", url: "https://inteltechniques.com/", descKey: "inteltechniques", tags: ["michael-bazzell", "methods", "training"] },
      { name: "Sector035", url: "https://sector035.nl/", descKey: "sector035", tags: ["weekly", "blog", "updates"] }
    ]
  }
];
```

## Mapeo de Iconos: JSX → String

| JSX Actual | String Equivalente | Icono Lucide |
|-----------|-------------------|-------------|
| `<Search className="w-5 h-5" />` | `"search"` | Lupa |
| `<MessageSquare className="w-5 h-5" />` | `"message-square"` | Chat |
| `<User className="w-5 h-5" />` | `"user"` | Persona |
| `<Map className="w-5 h-5" />` | `"map"` | Mapa |
| `<Briefcase className="w-5 h-5" />` | `"briefcase"` | Maletín |
| `<Activity className="w-5 h-5" />` | `"activity"` | Pulso |
| `<Cpu className="w-5 h-5" />` | `"cpu"` | Procesador |
| `<CheckCircle className="w-5 h-5" />` | `"check-circle"` | Check verde |

## Mapeo de Colores de Tags

El objeto completo de mapeo de tags a clases Tailwind (`getTagColor`). Contiene 80+ entradas:

```javascript
export function getTagColor(tag) {
  const colorMap = {
    // Search engines
    'general': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'popular': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'worldwide': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'advanced': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    'filters': 'bg-indigo-500/20 text-indigo-400 border border-indigo-500/30',
    'precise': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'security': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'dorks': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'vulnerabilities': 'bg-red-600/20 text-red-300 border border-red-600/30',
    'microsoft': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'alternative': 'bg-gray-500/20 text-gray-400 border border-gray-500/30',
    'images': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'russia': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'cyrillic': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'regional': 'bg-amber-500/20 text-amber-400 border border-amber-500/30',
    'china': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'chinese': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'privacy': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'anonymous': 'bg-green-600/20 text-green-300 border border-green-600/30',
    'no-tracking': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'google-proxy': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    // Social Media
    'twitter': 'bg-sky-500/20 text-sky-400 border border-sky-500/30',
    'analytics': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'statistics': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'bots': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'automation': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'detection': 'bg-yellow-500/20 text-yellow-400 border border-yellow-500/30',
    'archive': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'deleted': 'bg-gray-500/20 text-gray-400 border border-gray-500/30',
    'history': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'instagram': 'bg-pink-500/20 text-pink-400 border border-pink-500/30',
    'profiles': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'analysis': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'viewer': 'bg-pink-500/20 text-pink-400 border border-pink-500/30',
    'tiktok': 'bg-black/30 text-white border border-slate-500/30',
    'videos': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'search': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'telegram': 'bg-sky-500/20 text-sky-400 border border-sky-500/30',
    'channels': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'facebook': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    // Person Search
    'email': 'bg-amber-500/20 text-amber-400 border border-amber-500/30',
    'validation': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'reputation': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'gmail': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'google': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'accounts': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'finder': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'business': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'username': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'social': 'bg-pink-500/20 text-pink-400 border border-pink-500/30',
    'availability': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'comprehensive': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'platforms': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'breach': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'passwords': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'people': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'global': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'deep-web': 'bg-slate-500/20 text-slate-400 border border-slate-500/30',
    'reverse': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'phone': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'address': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    // Image & Mapping
    'image': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'face': 'bg-pink-500/20 text-pink-400 border border-pink-500/30',
    'recognition': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'ai': 'bg-violet-500/20 text-violet-400 border border-violet-500/30',
    'geolocation': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'shadow': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'time': 'bg-yellow-500/20 text-yellow-400 border border-yellow-500/30',
    'metadata': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'forensics': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'streetview': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'comparison': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'maps': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'satellite': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    'weather': 'bg-cyan-600/20 text-cyan-300 border border-cyan-600/30',
    'live': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'mountains': 'bg-amber-500/20 text-amber-400 border border-amber-500/30',
    'geography': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'identification': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    // Corporate & Tech
    'companies': 'bg-slate-500/20 text-slate-400 border border-slate-500/30',
    'corporate': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'database': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'panama-papers': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'offshore': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'financial': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'technology': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    'stack': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'website': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'url': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'domain': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'infrastructure': 'bg-orange-600/20 text-orange-300 border border-orange-600/30',
    'dns': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'encoding': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'decoding': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    // Transport & Live
    'ships': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'maritime': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'tracking': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'flights': 'bg-sky-500/20 text-sky-400 border border-sky-500/30',
    'aircraft': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    'railway': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'trains': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'radio': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'scanner': 'bg-red-600/20 text-red-300 border border-red-600/30',
    'emergency': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'webcams': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    // AI
    'openai': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'language': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'assistant': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'multimodal': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'anthropic': 'bg-purple-600/20 text-purple-300 border border-purple-600/30',
    'helpful': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'harmless': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'research': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'papers': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    'scientific': 'bg-green-600/20 text-green-300 border border-green-600/30',
    'humanizer': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'bypass': 'bg-red-500/20 text-red-400 border border-red-500/30',
    // Verification & Blogs
    'journalism': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'investigation': 'bg-red-500/20 text-red-400 border border-red-500/30',
    'verification': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'fact-check': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
    'debunking': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'rumors': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'directory': 'bg-blue-600/20 text-blue-300 border border-blue-600/30',
    'tools': 'bg-slate-500/20 text-slate-400 border border-slate-500/30',
    'michael-bazzell': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'methods': 'bg-orange-500/20 text-orange-400 border border-orange-500/30',
    'training': 'bg-green-500/20 text-green-400 border border-green-500/30',
    'weekly': 'bg-blue-500/20 text-blue-400 border border-blue-500/30',
    'blog': 'bg-purple-500/20 text-purple-400 border border-purple-500/30',
    'updates': 'bg-cyan-500/20 text-cyan-400 border border-cyan-500/30',
  };

  return colorMap[tag] || 'bg-slate-700/40 text-slate-300 border border-slate-600/30';
}
```

## Estadísticas del Catálogo

| Categoría | Herramientas | Tags Únicos |
|-----------|-------------|------------|
| searchEngines | 8 | 14 |
| socialMedia | 8 | 16 |
| personSearch | 9 | 18 |
| imageMapping | 7 | 14 |
| corporateTech | 6 | 12 |
| transportLive | 5 | 10 |
| artificialIntelligence | 5 | 10 |
| verificationBlogs | 5 | 10 |
| **Total** | **53** | **80+** |

## Notas de Migración

1. **IDCrawl** existe en `App.jsx` VeriSource_DATA pero NO en `osint-data.jsx`. Consolidar usando la versión de App.jsx.
2. Los iconos de categoría se definían como JSX (`<Search className="w-5 h-5" />`). En vanilla se referencian por nombre string (`"search"`).
3. Los `descKey` se usan como claves de traducción: `t('tools.' + descKey)`.
4. Los `tags` se usan para:
   - Filtrado de búsqueda
   - Color-coding visual con `getTagColor()`
   - Se muestran máximo 2 tags por tarjeta
