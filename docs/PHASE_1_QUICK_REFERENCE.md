# LOLOWEBDEV v2.0 - Phase 1: Quick Reference
**Foundation Setup** | Audit + Design Tokens + Video Infrastructure

---

## 🎯 Phase 1 Objetivos

✅ Auditar UX actual (Lighthouse, Core Web Vitals)  
✅ Crear design tokens v2.0  
✅ Estructurar carpetas para videos  

**Status**: 🟢 In Progress  
**Timeline**: Semana 1-2 (Mayo 2026)

---

## 📋 Checklist de Tareas

### Tarea 1: Audit UX Actual

#### 1.1 Lighthouse Score
```bash
# Ejecutar en staging/producción
npm run build
npm run audit
# Target: 98+ (v1.0 fue 92)
```

**Métricas a revisar**:
- Performance: 90+ (fue 87)
- Accessibility: 95+ (fue 93)
- Best Practices: 95+ (fue 94)
- SEO: 90+ (fue 92)

#### 1.2 Core Web Vitals
```javascript
// Instrumentación en el sitio
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

getCLS(console.log); // Cumulative Layout Shift < 0.1
getFID(console.log); // First Input Delay < 100ms
getLCP(console.log); // Largest Contentful Paint < 2.5s
```

**Targets**:
| Métrica | v1.0 | v2.0 |
|---------|------|------|
| **CLS** | 0.08 | 0.05 |
| **FID** | 95ms | < 100ms |
| **LCP** | 2.2s | < 2.5s |
| **TTL** | 1.8s | < 1.5s |

#### 1.3 Accessibility Deep Dive
```bash
# Herramientas
- axe DevTools (Chrome Extension)
- WAVE (WebAIM)
- Lighthouse
- Manual keyboard navigation (Tab through all sections)

# Validaciones
- [ ] Contrast ratios AAA (7:1 mínimo)
- [ ] Focus rings visibles en todos los elementos interactivos
- [ ] ARIA labels en botones sin texto
- [ ] Keyboard navigation lógica (tab order)
- [ ] Reduced motion detection: prefers-reduced-motion works
```

---

### Tarea 2: Design Tokens v2.0

#### 2.1 Crear archivo de tokens
```bash
# Crear archivo TypeScript
touch src/design-tokens.ts
```

#### 2.2 Estructura base
```typescript
// src/design-tokens.ts
export const DESIGN_TOKENS = {
  // Colores primarios (por dominio)
  colors: {
    // SaaS B2B
    saas: {
      primary: '#2E4036',    // Verde musgo
      accent: '#CC5833',     // Arcilla
      surface: '#F5F3F0',    // Crema
      text: '#1a1a1a',
      textMuted: '#666666',
    },
    // Agencia Creativa
    agency: {
      primary: '#D4A574',    // Dorado
      accent: '#1a1a1a',     // Negro
      surface: '#F5F3F0',    // Crema
      text: '#1a1a1a',
    },
    // Startup Pre-Seed
    startup: {
      primary: '#00FF88',    // Neon
      accent: '#9D4EDD',     // Púrpura
      surface: '#1F1F1F',    // Charcoal
      text: '#FFFFFF',
    },
  },
  
  // Espaciado
  spacing: {
    xs: '0.25rem',   // 4px
    sm: '0.5rem',    // 8px
    md: '1rem',      // 16px
    lg: '2rem',      // 32px
    xl: '4rem',      // 64px
    xxl: '6rem',     // 96px
  },
  
  // Tipografía
  typography: {
    fontSize: {
      xs: '0.75rem',
      sm: '0.875rem',
      base: '1rem',
      lg: '1.125rem',
      xl: '1.25rem',
      '2xl': '1.5rem',
      '3xl': '2rem',
      '4xl': '2.5rem',
      '5xl': '3rem',
      '6xl': '3.75rem',
    },
    fontFamily: {
      display: "'Plus Jakarta Sans', sans-serif",
      heading: "'Outfit', sans-serif",
      body: "'Inter', sans-serif",
      mono: "'IBM Plex Mono', monospace",
    },
    fontWeight: {
      regular: 400,
      medium: 500,
      semibold: 600,
      bold: 700,
    },
    lineHeight: {
      tight: 1.1,
      normal: 1.5,
      relaxed: 1.8,
    },
  },
  
  // Transiciones
  transitions: {
    fast: '150ms cubic-bezier(0.4, 0, 0.2, 1)',
    standard: '300ms cubic-bezier(0.4, 0, 0.2, 1)',
    slow: '500ms cubic-bezier(0.2, 0, 0.2, 1)',
  },
  
  // Sombras
  shadows: {
    sm: '0 1px 2px rgba(0, 0, 0, 0.05)',
    md: '0 4px 6px rgba(0, 0, 0, 0.1)',
    lg: '0 10px 15px rgba(0, 0, 0, 0.1)',
    elevation: '0 8px 32px rgba(0, 0, 0, 0.1)',
  },
  
  // Radios
  radius: {
    sm: '4px',
    md: '8px',
    lg: '12px',
    xl: '16px',
    full: '9999px',
  },
};

// CSS Custom Properties
export const cssVariables = `
:root {
  /* Colores */
  --color-primary: #2E4036;
  --color-accent: #CC5833;
  --color-surface: #F5F3F0;
  --color-text-primary: #1a1a1a;
  --color-text-secondary: #666666;
  
  /* Tipografía */
  --font-display: 'Plus Jakarta Sans', sans-serif;
  --font-heading: 'Outfit', sans-serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
  
  /* Espaciado */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;
  --spacing-xl: 4rem;
  
  /* Transiciones */
  --transition-fast: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-standard: 300ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-slow: 500ms cubic-bezier(0.2, 0, 0.2, 1);
  
  /* Sombras */
  --shadow-elevation: 0 8px 32px rgba(0, 0, 0, 0.1);
}
`;
```

#### 2.3 Integrar en global.css
```css
/* src/styles/global.css */
:root {
  --color-primary: #2E4036;
  --color-accent: #CC5833;
  --color-surface: #F5F3F0;
  --color-text-primary: #1a1a1a;
  --color-text-secondary: #666666;
  
  --font-display: 'Plus Jakarta Sans', sans-serif;
  --font-heading: 'Outfit', sans-serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
  
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;
  --spacing-xl: 4rem;
  
  --transition-fast: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-standard: 300ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-slow: 500ms cubic-bezier(0.2, 0, 0.2, 1);
  
  --shadow-elevation: 0 8px 32px rgba(0, 0, 0, 0.1);
}

/* Aplicar tipografía base */
body {
  font-family: var(--font-body);
  color: var(--color-text-primary);
  line-height: 1.6;
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-heading);
  font-weight: 600;
  line-height: 1.1;
}

/* Componentes comunes */
button {
  transition: all var(--transition-standard);
  border-radius: var(--spacing-md);
  padding: var(--spacing-md) var(--spacing-lg);
}

button:hover {
  box-shadow: var(--shadow-elevation);
}
```

---

### Tarea 3: Estructura de Carpetas Video

```bash
# Crear estructura
mkdir -p public/videos/{hero,features,testimonials,pricing,misc}
mkdir -p public/videos/posters
mkdir -p public/videos/frames

# Subfolders
public/videos/
├── hero/
│   ├── hero-main.mp4         # Video principal hero
│   ├── hero-main.webm        # Fallback format
│   └── hero-poster.jpg       # Static poster frame
├── features/
│   ├── feature-1-demo.mp4
│   ├── feature-2-demo.mp4
│   ├── feature-3-demo.mp4
│   └── [etc]
├── testimonials/
│   ├── customer-1.mp4        # 5-8s clip
│   ├── customer-2.mp4
│   └── [etc]
├── pricing/
│   ├── pricing-comparison.mp4
│   └── 3d-animation.mp4
├── misc/
│   └── [other videos]
├── posters/
│   └── [all poster frames]
└── frames/
    └── [canvas-rendered frames if needed]
```

#### 3.1 Video Optimization Guide
```bash
# Comprimir videos para web
# MP4 (H.264)
ffmpeg -i input.mov -c:v libx264 -preset medium -crf 22 -c:a aac -b:a 128k output.mp4

# WebM (VP9 - mejor compresión)
ffmpeg -i input.mov -c:v libvpx-vp9 -crf 30 -b:v 0 -c:a libopus output.webm

# Poster frame
ffmpeg -i input.mp4 -ss 00:00:01 -vframes 1 -q:v 5 poster.jpg
```

#### 3.2 Video HTML Template
```html
<!-- Hero video -->
<video autoplay muted loop playsinline poster="/videos/posters/hero-poster.jpg">
  <source src="/videos/hero/hero-main.mp4" type="video/mp4">
  <source src="/videos/hero/hero-main.webm" type="video/webm">
  Your browser does not support HTML5 video.
</video>

<!-- Feature video with controls -->
<video controls poster="/videos/posters/feature-1-poster.jpg">
  <source src="/videos/features/feature-1-demo.mp4" type="video/mp4">
  <source src="/videos/features/feature-1-demo.webm" type="video/webm">
</video>

<!-- Testimonial video with play button -->
<div class="video-container">
  <video 
    playsinline 
    poster="/videos/posters/testimonial-1-poster.jpg"
    data-video-id="testimonial-1"
  >
    <source src="/videos/testimonials/customer-1.mp4" type="video/mp4">
    <source src="/videos/testimonials/customer-1.webm" type="video/webm">
  </video>
  <button class="play-button" aria-label="Play video"></button>
</div>
```

---

## 📚 Documentación Integrada

✅ **lolowebdev-enhanced.md** — Arquitectura v2.0 completa  
✅ **lolowebdev-ux-guidelines.md** — Base de datos UX (estilos, paletas, tipografía)  
✅ **lolowebdev-video-integration.md** — Lenis + GSAP + Canvas rendering  
✅ **lolowebdev-domain-templates.md** — Templates por dominio (SaaS, Agency, Startup, E-commerce)  
✅ **PHASE_1_QUICK_REFERENCE.md** — Este documento  

---

## 🚀 Próximos Pasos (Phase 2)

Una vez completado Phase 1:

**Fase 2: UX Enhancements (Semana 3-4)**
- [ ] Refinar jerarquía visual (h1 hasta 72px en hero)
- [ ] Implementar micro-interacciones (button states, skeleton loaders)
- [ ] Completar accesibilidad (focus rings, ARIA labels)
- [ ] Validar con Lighthouse 98+

**Fase 3: Video Integration (Semana 5-6)**
- [ ] Producir/compilar videos (hero, features, testimonios)
- [ ] Integrar componentes video
- [ ] Optimizar rendimiento (lazy-load, CDN)

**Fase 4: Testing + Polish (Semana 7-8)**
- [ ] Cross-browser testing
- [ ] Mobile performance audit
- [ ] A/B testing (con/sin video)

---

## 🎯 Success Criteria Phase 1

✅ Lighthouse 98+ (all categories)  
✅ Core Web Vitals Excellent (CLS < 0.05, LCP < 2.5s)  
✅ WCAG AAA accessibility (7:1 contrast minimum)  
✅ Design tokens v2.0 implemented  
✅ Video folder structure ready  
✅ Documentation complete (4 docs + this reference)  

---

**Status**: 🟢 Ready to Implement  
**Estimated Completion**: Mayo 24-31, 2026  
**Owner**: LOLOWEBDEV Enhancement Task Force

