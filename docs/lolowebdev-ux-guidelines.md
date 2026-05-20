# LOLOWEBDEV UX Guidelines Database
**Referencia Integrada**: UI/UX Intelligence Search System

---

## 📚 ÍNDICE

1. [Estilos UI](#estilos-ui)
2. [Paletas de Color por Dominio](#paletas-de-color)
3. [Tipografía Estratégica](#tipografía)
4. [UX Best Practices](#ux-best-practices)
5. [Tokens de Diseño v2.0](#tokens)

---

## 🎨 Estilos UI

### Glassmorphism (Moderno + Accesible)
**Cuándo**: SaaS premium, fintech, apps modernas  
**Características**: Fondo desenfocado, bordes semi-transparentes, contraste elevado  
**CSS Pattern**:
```css
.glass-card {
  background: rgba(255, 255, 255, 0.10);
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}
```
**Color Harmonies**: Blanco + transparencia + color accent (max 1 accent por sección)  
**Motion**: Fade-in smooth, escala suave (scale 0.95 → 1.0)  
**AI Prompt Keywords**: "frosted glass", "blurred background", "modern web", "minimalist luxury"

---

### Minimalism (Data-Forward)
**Cuándo**: SaaS B2B, startups, dashboards  
**Características**: Espacios en blanco amplios, tipografía jerárquica, máximo 2 colores  
**CSS Pattern**:
```css
.minimal-card {
  background: #ffffff;
  border: none;
  padding: 3rem 2rem;
  border-left: 4px solid var(--accent);
}
```
**Color Harmonies**: Monocromía base + 1 accent color + variaciones de grises  
**Motion**: Micro-delays (stagger: 0.1s), entrances suaves  
**AI Prompt Keywords**: "clean lines", "whitespace", "minimalist", "focus on content", "data-driven"

---

### Brutalism (Provocador + Editorial)
**Cuándo**: Agencias creativas, editorial, startups boldas  
**Características**: Tipografía grande, bordes duros, sin refinamiento aparente, alto contraste  
**CSS Pattern**:
```css
.brutalist-section {
  background: #000;
  color: #fff;
  border: 3px solid #fff;
  padding: 4rem;
  font-size: clamp(2rem, 10vw, 5rem);
  letter-spacing: -0.02em;
}
```
**Color Harmonies**: Blanco + negro + 1 color saturado (rojo, amarillo, azul puro)  
**Motion**: Jump cuts, clip-path reveals, sin suavidad  
**AI Prompt Keywords**: "bold", "raw", "editorial", "high contrast", "large typography"

---

### Luxury Modern (Premium + Refinado)
**Cuándo**: Agencias de lujo, joyería, real estate premium  
**Características**: Serif elegante, espacios generosos, detalles dorados, animaciones lentas  
**CSS Pattern**:
```css
.luxury-card {
  font-family: 'Cormorant Garamond', serif;
  background: #f5f3f0;
  border-top: 2px solid #d4a574;
  padding: 4rem;
  line-height: 1.8;
  letter-spacing: 0.05em;
}
```
**Color Harmonies**: Monocromía cálida (beiges, creams) + oro/cobre + acento oscuro  
**Motion**: Transiciones 400ms+, scale suave, entrada desde abajo  
**AI Prompt Keywords**: "elegant", "refined", "luxury", "gold accents", "serif", "premium"

---

### Tech Premium (Futurista + Funcional)
**Cuándo**: SaaS, fintech, tech startups  
**Características**: Gradientes sutiles, líneas geométricas, animaciones dinámicas, mono-space data  
**CSS Pattern**:
```css
.tech-card {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'IBM Plex Mono', monospace;
  border-radius: 8px;
  padding: 2rem;
  position: relative;
  overflow: hidden;
}
```
**Color Harmonies**: Gradiente (2-3 colores), vibración neon opcional, oscuro base  
**Motion**: Rotate, scale-y, entrada desde costado con aceleración  
**AI Prompt Keywords**: "tech", "futuristic", "gradient", "dynamic", "modern tech"

---

### Organic Future (Sostenible + Curvilíneo)
**Cuándo**: Eco, wellness, startups sostenibles  
**Características**: Bordes redondeados, colores naturales, formas amorfas, movimiento fluido  
**CSS Pattern**:
```css
.organic-card {
  background: #d4e8d4;
  border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%;
  padding: 2rem;
  box-shadow: -5px 10px 30px rgba(0, 0, 0, 0.08);
}
```
**Color Harmonies**: Verdes + azules + terracota + beige natural  
**Motion**: Morph shapes, undulating, entrances fluidas  
**AI Prompt Keywords**: "organic", "natural", "eco", "flowing", "sustainable"

---

## 🎨 Paletas de Color por Dominio

### SaaS B2B (Tech Premium)
**Primarios**: Verde musgo (#2E4036) + Arcilla (#CC5833) + Crema (#F5F3F0)  
**Secundarios**: Gris cálido (#A89F99), Azul confianza (#5B8DBE)  
**Accents**: Verde musgo para CTAs primarias, arcilla para advertencias  
**Uso**: Headings verde musgo, body gris cálido, CTAs arcilla  
**WCAG**: AAA completo (contraste 7:1+ en CTAs)

**Caso de uso**: 
```jsx
// Palette SaaS
const colors = {
  primary: '#2E4036',    // Verde musgo - headings, CTAs
  secondary: '#CC5833',  // Arcilla - highlights, warnings
  background: '#F5F3F0', // Crema - fondo
  textPrimary: '#1a1a1a',// Negro - body
  textSecondary: '#A89F99', // Gris - labels, captions
};
```

---

### Agencia Creativa (Lujo Moderno)
**Primarios**: Dorado (#D4A574) + Negro (#1a1a1a) + Blanco roto (#F5F3F0)  
**Secundarios**: Champagne (#E8D7C3), Gris grafito (#44403C)  
**Accents**: Dorado para highlights, negro para énfasis  
**Uso**: Tipografía serif dorada para headings, negro para body  
**WCAG**: AAA en combinaciones negra + blanco

---

### Startup Pre-Seed (Minimalismo Oscuro)
**Primarios**: Neon (#00FF88) + Gris charcoal (#1F1F1F) + Blanco (#FFFFFF)  
**Secundarios**: Púrpura neon (#9D4EDD), Gris claro (#E0E0E0)  
**Accents**: Neon verde para CTAs, púrpura para features  
**Uso**: Alto contraste, máxima legibilidad, movimiento sutil  
**WCAG**: AAA garantizado (neon sobre oscuro = 10:1+)

---

### E-commerce Luxury
**Paleta Monocroma**: Gradiente de un color (ej. Negro: #000 → #333 → #666)  
O **Gradiente Saturado**: Magenta (#FF1493) → Violeta (#8B00FF) → Azul (#0066FF)  
**Secondarios**: Oro (#FFD700) para badges, Blanco (#FFF) para CTAs  
**Accents**: Oro para "premium", blanco para "buy now"  
**Uso**: Cada producto color único dentro de la monocromía  
**WCAG**: AAA en CTAs principales

---

### Startup Energía Vibrante
**Primarios**: Rojo (#FF3B30) + Naranja (#FF9500) + Amarillo (#FFCC00)  
**Secundarios**: Verde (#34C759), Azul (#00B4D8)  
**Accents**: Cada sección color diferente (rainbow gradient)  
**Uso**: Alto contraste, máxima energía, juventud  
**WCAG**: AAA en negro body text

---

## 📝 Tipografía Estratégica

### Hierarchy System

| Rol | Familia | Peso | Tamaño Móvil | Tamaño Desktop | Línea | Letra |
|-----|---------|------|--------------|----------------|-------|-------|
| **H1 Hero** | Plus Jakarta Sans | Bold (700) | 2.5rem | 5rem+ | 1.1 | -0.02em |
| **H1 Premium** | Cormorant Garamond | Regular (400) | 2rem | 4rem | 1.2 | 0.05em |
| **H2/H3** | Outfit | SemiBold (600) | 1.5rem | 2.5rem | 1.3 | -0.01em |
| **Body** | Inter | Regular (400) | 1rem | 1.125rem | 1.6 | 0 |
| **Data/Metrics** | IBM Plex Mono | Regular (400) | 0.875rem | 1rem | 1.4 | 0 |
| **Captions** | Inter | Regular (400) | 0.75rem | 0.875rem | 1.4 | 0.01em |

### Google Fonts Imports
```css
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700&family=Outfit:wght@400;600;700&family=Cormorant+Garamond:ital@0;1&family=IBM+Plex+Mono:wght@400;700&family=Inter:wght@400;500;600;700&display=swap');
```

### Pairings Recomendados
- **Tech Premium**: Plus Jakarta Sans (headings) + Inter (body)
- **Luxury Modern**: Cormorant Garamond (headings serif) + Inter (body)
- **Minimalist**: Outfit (headings) + Inter (body)
- **Data-Heavy**: IBM Plex Mono (metrics) + Inter (body)
- **Brutalist**: Outfit Bold (headings massive) + Inter (body monoline)

---

## ✅ UX Best Practices

### Contrast & Accessibility
- [ ] **Contrast Ratios**: AAA mínimo (7:1 para body, 4.5:1 para UI)
- [ ] **Focus Rings**: 4px offset, color accent, visible siempre
- [ ] **Keyboard Navigation**: Tab order coherente, skip links
- [ ] **Screen Reader**: ARIA labels en botones, `role="region"` en secciones
- [ ] **Color Blindness**: No usar rojo/verde solos, añadir patrón/icono

### Motion & Animation
- [ ] **Reduced Motion**: Detectar `prefers-reduced-motion`, deshabilitar GSAP
- [ ] **Transitions**: 150ms (fast), 300ms (standard), 500ms (slow)
- [ ] **Easing**: `cubic-bezier(0.4, 0, 0.2, 1)` estándar
- [ ] **Stagger**: 0.1s-0.15s entre elementos
- [ ] **No Flashing**: < 3 parpadeos/segundo

### Componentes Premium
- **Buttons**: 4 estados (default, hover, active, disabled) con feedback visual
- **Cards**: Elevation simulada con `box-shadow: 0 8px 32px rgba(0,0,0,0.1)`
- **Loaders**: Skeleton animations elegantes, nunca spinners baratos
- **Toasts**: Slide-in superior + fade-out 3s, no obstrusivos
- **Modals**: Blur backdrop, entrance scale 0.95 → 1.0

### Spacing
- **Mobile**: 1.5rem gaps, 1rem padding interno
- **Tablet**: 2rem gaps, 1.5rem padding interno
- **Desktop**: 4rem gaps, 2rem padding interno

---

## 🎯 Tokens de Diseño v2.0

### Color Tokens
```typescript
export const DESIGN_TOKENS = {
  colors: {
    // Primarios
    primary: {
      50: '#F0F4F3',
      100: '#D9E5E2',
      200: '#B8D1CB',
      500: '#2E4036', // Base
      900: '#1a2622',
    },
    accent: {
      50: '#FEF4F1',
      100: '#F8D9CF',
      500: '#CC5833', // Base
      900: '#7D3020',
    },
    // Neutral
    surface: '#F5F3F0',
    background: '#FFFFFF',
    border: '#E5E0D8',
    textPrimary: '#1a1a1a',
    textSecondary: '#666666',
    textMuted: '#999999',
  },
  
  // Spacing (4px base)
  spacing: {
    xs: '0.25rem',   // 4px
    sm: '0.5rem',    // 8px
    md: '1rem',      // 16px
    lg: '2rem',      // 32px
    xl: '4rem',      // 64px
    xxl: '6rem',     // 96px
  },
  
  // Typography
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
    letterSpacing: {
      tight: '-0.02em',
      normal: '0',
      wide: '0.05em',
    },
  },
  
  // Transitions
  transitions: {
    fast: '150ms cubic-bezier(0.4, 0, 0.2, 1)',
    standard: '300ms cubic-bezier(0.4, 0, 0.2, 1)',
    slow: '500ms cubic-bezier(0.2, 0, 0.2, 1)',
  },
  
  // Shadows
  shadows: {
    sm: '0 1px 2px rgba(0, 0, 0, 0.05)',
    md: '0 4px 6px rgba(0, 0, 0, 0.1)',
    lg: '0 10px 15px rgba(0, 0, 0, 0.1)',
    xl: '0 20px 25px rgba(0, 0, 0, 0.1)',
    elevation: '0 8px 32px rgba(0, 0, 0, 0.1)',
  },
  
  // Border Radius
  radius: {
    sm: '4px',
    md: '8px',
    lg: '12px',
    xl: '16px',
    full: '9999px',
  },
};
```

### CSS Custom Properties
```css
:root {
  /* Colors */
  --color-primary: #2E4036;
  --color-accent: #CC5833;
  --color-surface: #F5F3F0;
  --color-text-primary: #1a1a1a;
  --color-text-secondary: #666666;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;
  --spacing-xl: 4rem;
  
  /* Typography */
  --font-display: 'Plus Jakarta Sans', sans-serif;
  --font-heading: 'Outfit', sans-serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
  
  /* Transitions */
  --transition-fast: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-standard: 300ms cubic-bezier(0.4, 0, 0.2, 1);
  --transition-slow: 500ms cubic-bezier(0.2, 0, 0.2, 1);
  
  /* Shadows */
  --shadow-elevation: 0 8px 32px rgba(0, 0, 0, 0.1);
  --shadow-hover: 0 12px 40px rgba(0, 0, 0, 0.15);
}
```

---

## 🔍 Búsqueda Rápida

**"Quiero un estilo moderno y premium"** → Glassmorphism + Tech Premium palette + Plus Jakarta Sans

**"Necesito máximo contraste y energía"** → Brutalism + Neon palette + Outfit Bold

**"Estoy haciendo un sitio de lujo"** → Luxury Modern + Dorado/Negro palette + Cormorant Garamond

**"SaaS B2B profesional"** → Minimalism + Verde musgo palette + Inter + IBM Plex Mono

**"Startup joven y sostenible"** → Organic Future + Verdes/Azules naturales + Outfit

---

**Status**: ✅ Integrada  
**Last Updated**: Mayo 2026  
**Source**: ui-ux-pro-max-skill + lolowebdev-enhanced.md
