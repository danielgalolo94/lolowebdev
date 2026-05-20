# LOLOWEBDEV v2.0 - Phase 2: UX Enhancements
**Jerarquía Visual + Micro-interacciones + Accesibilidad Avanzada**

---

## 🎯 Phase 2 Objetivos

✅ Refinar jerarquía visual (tipografía dinámica, espaciado progresivo)  
✅ Implementar micro-interacciones premium (4 estados por componente)  
✅ Completar accesibilidad (focus rings, reduced-motion, ARIA)  
✅ Validar Lighthouse 98+ en proyecto real  

**Timeline**: Semana 3-4  
**Status**: 🟡 Ready to Start  
**Dependencies**: Phase 1 completado ✅

---

## 📋 Tarea 1: Jerarquía Visual Mejorada

### 1.1 Tipografía Dinámica (clamp)

Usar `clamp()` para tipografía fluida responsiva:

```css
/* src/styles/typography.css */

:root {
  /* Responsive hero heading */
  --h1-size: clamp(2.5rem, 10vw, 5rem);
  --h1-line-height: 1.1;
  --h1-letter-spacing: -0.02em;
  
  /* Section headings */
  --h2-size: clamp(1.5rem, 6vw, 2.5rem);
  --h2-line-height: 1.2;
  --h2-letter-spacing: -0.01em;
  
  /* Body text */
  --body-size: clamp(0.875rem, 2vw, 1.125rem);
  --body-line-height: 1.6;
  
  /* Data/metrics (monospace) */
  --mono-size: clamp(0.75rem, 1.5vw, 1rem);
  --mono-line-height: 1.4;
}

h1 {
  font-size: var(--h1-size);
  font-weight: 700;
  line-height: var(--h1-line-height);
  letter-spacing: var(--h1-letter-spacing);
  margin-bottom: clamp(1rem, 3vw, 2rem);
}

h2 {
  font-size: var(--h2-size);
  font-weight: 600;
  line-height: var(--h2-line-height);
  letter-spacing: var(--h2-letter-spacing);
  margin-bottom: clamp(0.75rem, 2vw, 1.5rem);
}

body {
  font-size: var(--body-size);
  line-height: var(--body-line-height);
  color: var(--color-text-primary);
}

code, .mono {
  font-family: var(--font-mono);
  font-size: var(--mono-size);
  line-height: var(--mono-line-height);
}
```

### 1.2 Espaciado Progresivo por Viewport

```css
/* Spacing scales dinámicamente por viewport */
:root {
  /* Sections gap */
  --section-gap: clamp(1.5rem, 5vw, 4rem);
  
  /* Card padding */
  --card-padding: clamp(1rem, 2vw, 2rem);
  
  /* Margin bottom */
  --mb-sm: clamp(0.5rem, 1vw, 1rem);
  --mb-md: clamp(1rem, 2vw, 2rem);
  --mb-lg: clamp(1.5rem, 3vw, 3rem);
}

section {
  margin-bottom: var(--section-gap);
  padding: var(--card-padding);
}

.card {
  gap: var(--section-gap);
  padding: var(--card-padding);
}
```

### 1.3 Contrast Audit & Validation

```bash
# Usar herramientas para validar WCAG AAA (7:1 minimum)
# Tools: axe DevTools, WAVE, Lighthouse

# Validación por color
Colors necesarios:
├── Primary text on light: #1a1a1a on #F5F3F0 = 16:1 ✅
├── Secondary text: #666666 on #F5F3F0 = 7.2:1 ✅
├── Body text: #1a1a1a on #FFFFFF = 21:1 ✅
├── Accent button: #FFFFFF on #CC5833 = 5.2:1 ✅
├── Focus ring: #2E4036 on #F5F3F0 = 12:1 ✅
└── Disabled state: #999999 on #F5F3F0 = 4.5:1 ⚠️ (necesita ajuste)
```

### 1.4 Validation Checklist

```html
<!-- Estructura semántica correcta -->
<h1>Página principal</h1>  <!-- Solo 1 h1 por página -->
<section>
  <h2>Sección</h2>
  <p>Body text...</p>
</section>

<!-- Contrast ratios validados -->
<button style="background: var(--color-accent); color: white;">
  <!-- Validar: 5.2:1 ratio = AA completo ✅ -->
  Click me
</button>

<!-- Disabled state con sufficient contrast -->
<button disabled style="color: #666666;">
  <!-- Validar: 7.2:1 ratio = AAA ✅ -->
  Disabled Button
</button>
```

---

## 📋 Tarea 2: Micro-interacciones Premium

### 2.1 Button States (4 estados)

```tsx
// src/components/Button.tsx
import styles from './Button.module.css';

interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  disabled?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

export function Button({ 
  variant = 'primary', 
  disabled = false, 
  children,
  onClick 
}: ButtonProps) {
  return (
    <button
      className={`${styles.button} ${styles[variant]} ${disabled ? styles.disabled : ''}`}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

```css
/* src/components/Button.module.css */

.button {
  padding: var(--spacing-md) var(--spacing-lg);
  font-size: 1rem;
  font-weight: 600;
  border: none;
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: all var(--transition-standard);
  position: relative;
  overflow: hidden;
}

/* PRIMARY VARIANT */
.primary {
  background: var(--color-primary);
  color: white;
}

/* State: Default */
.primary:not(:disabled) {
  box-shadow: 0 2px 8px rgba(46, 64, 54, 0.15);
}

/* State: Hover */
.primary:not(:disabled):hover {
  background: #3a4f45;
  box-shadow: 0 8px 24px rgba(46, 64, 54, 0.25);
  transform: translateY(-2px);
}

/* State: Active/Focus */
.primary:not(:disabled):active {
  transform: translateY(0);
  box-shadow: 0 4px 12px rgba(46, 64, 54, 0.15);
}

.primary:not(:disabled):focus-visible {
  outline: none;
  box-shadow: 
    0 4px 12px rgba(46, 64, 54, 0.15),
    0 0 0 4px var(--color-surface),
    0 0 0 6px var(--color-primary);
}

/* State: Disabled */
.primary:disabled {
  background: #D9D5D0;
  color: #999999;
  cursor: not-allowed;
  box-shadow: none;
}

/* SECONDARY VARIANT */
.secondary {
  background: transparent;
  color: var(--color-primary);
  border: 2px solid var(--color-primary);
}

.secondary:not(:disabled):hover {
  background: var(--color-primary);
  color: white;
  box-shadow: 0 8px 24px rgba(46, 64, 54, 0.15);
  transform: translateY(-2px);
}

/* GHOST VARIANT */
.ghost {
  background: transparent;
  color: var(--color-primary);
  box-shadow: none;
}

.ghost:not(:disabled):hover {
  background: rgba(46, 64, 54, 0.05);
  box-shadow: none;
}
```

### 2.2 Card Elevation (3D Shadow Gradients)

```css
/* src/styles/cards.css */

.card {
  background: white;
  border-radius: var(--radius-lg);
  padding: var(--card-padding);
  transition: all var(--transition-standard);
  
  /* Base shadow */
  box-shadow: 
    0 1px 3px rgba(0, 0, 0, 0.08),
    0 4px 12px rgba(0, 0, 0, 0.08);
}

/* State: Hover (elevated) */
.card:hover {
  transform: translateY(-4px);
  box-shadow: 
    0 2px 8px rgba(0, 0, 0, 0.12),
    0 12px 32px rgba(0, 0, 0, 0.15);
}

/* State: Active (clicked) */
.card:active {
  transform: translateY(-2px);
  box-shadow: 
    0 1px 4px rgba(0, 0, 0, 0.1),
    0 8px 24px rgba(0, 0, 0, 0.12);
}

/* Alternative: Glassmorphism card */
.card-glass {
  background: rgba(255, 255, 255, 0.10);
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.25);
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.1),
    inset 0 1px 1px rgba(255, 255, 255, 0.25);
}

.card-glass:hover {
  box-shadow: 
    0 16px 64px rgba(0, 0, 0, 0.15),
    inset 0 1px 1px rgba(255, 255, 255, 0.3);
}
```

### 2.3 Skeleton Loaders (Animaciones Elegantes)

```tsx
// src/components/SkeletonLoader.tsx

import styles from './SkeletonLoader.module.css';

interface SkeletonProps {
  width?: string;
  height?: string;
  variant?: 'text' | 'circle' | 'rect';
}

export function Skeleton({ 
  width = '100%', 
  height = '1rem',
  variant = 'text' 
}: SkeletonProps) {
  return (
    <div
      className={`${styles.skeleton} ${styles[variant]}`}
      style={{ width, height }}
      aria-busy="true"
      aria-label="Loading content"
    />
  );
}

// Ejemplo de uso
export function SkeletonCard() {
  return (
    <div className={styles.card}>
      <Skeleton height="200px" variant="rect" />
      <Skeleton width="80%" height="1.5rem" variant="text" style={{ marginTop: '1rem' }} />
      <Skeleton width="100%" height="1rem" variant="text" style={{ marginTop: '0.5rem' }} />
      <Skeleton width="60%" height="1rem" variant="text" />
    </div>
  );
}
```

```css
/* src/components/SkeletonLoader.module.css */

.skeleton {
  background: linear-gradient(
    90deg,
    var(--color-surface) 0%,
    rgba(229, 224, 216, 0.5) 50%,
    var(--color-surface) 100%
  );
  background-size: 200% 100%;
  animation: shimmer 2s infinite;
  border-radius: var(--radius-sm);
}

.text {
  border-radius: var(--radius-sm);
}

.circle {
  border-radius: 50%;
}

.rect {
  border-radius: var(--radius-lg);
}

@keyframes shimmer {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}

.card {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: var(--card-padding);
}
```

### 2.4 Toast Notifications (Slide + Fade)

```tsx
// src/components/Toast.tsx

import { useEffect } from 'react';
import styles from './Toast.module.css';

interface ToastProps {
  message: string;
  type?: 'success' | 'error' | 'warning' | 'info';
  duration?: number;
  onClose?: () => void;
}

export function Toast({ 
  message, 
  type = 'info', 
  duration = 3000,
  onClose 
}: ToastProps) {
  useEffect(() => {
    const timer = setTimeout(() => {
      onClose?.();
    }, duration);
    return () => clearTimeout(timer);
  }, [duration, onClose]);

  return (
    <div className={`${styles.toast} ${styles[type]}`} role="alert">
      {type === 'success' && '✓'}
      {type === 'error' && '✕'}
      {type === 'warning' && '⚠'}
      {type === 'info' && 'ℹ'}
      
      <span>{message}</span>
    </div>
  );
}
```

```css
/* src/components/Toast.module.css */

.toast {
  position: fixed;
  top: 2rem;
  right: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem 1.5rem;
  border-radius: var(--radius-lg);
  background: white;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
  font-weight: 500;
  
  /* Animation */
  animation: slideInRight var(--transition-standard) ease-out,
            slideOutRight var(--transition-standard) ease-in 2.7s forwards;
}

.success {
  border-left: 4px solid #4CAF50;
  color: #4CAF50;
}

.error {
  border-left: 4px solid #F44336;
  color: #F44336;
}

.warning {
  border-left: 4px solid #FF9800;
  color: #FF9800;
}

.info {
  border-left: 4px solid #2196F3;
  color: #2196F3;
}

@keyframes slideInRight {
  from {
    transform: translateX(400px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes slideOutRight {
  from {
    transform: translateX(0);
    opacity: 1;
  }
  to {
    transform: translateX(400px);
    opacity: 0;
  }
}
```

---

## 📋 Tarea 3: Accesibilidad Avanzada

### 3.1 Focus Rings Styled

```css
/* src/styles/accessibility.css */

/* Focus visible para teclado */
button:focus-visible,
a:focus-visible,
input:focus-visible,
textarea:focus-visible,
select:focus-visible {
  outline: none;
  box-shadow: 0 0 0 4px var(--color-surface),
              0 0 0 6px var(--color-primary);
}

/* Skip link (para screen readers) */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: var(--color-primary);
  color: white;
  padding: 0.5rem 1rem;
  text-decoration: none;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}

/* Focus visible outline */
:focus-visible {
  outline: 3px solid var(--color-accent);
  outline-offset: 2px;
}
```

### 3.2 Reduced Motion Detection

```typescript
// src/hooks/useReducedMotion.ts

export function useReducedMotion(): boolean {
  if (typeof window === 'undefined') return false;
  
  const mediaQuery = window.matchMedia('(prefers-reduced-motion: reduce)');
  return mediaQuery.matches;
}

// Uso en componentes
import { useReducedMotion } from '@/hooks/useReducedMotion';

export function AnimatedComponent() {
  const prefersReducedMotion = useReducedMotion();
  
  return (
    <div
      style={{
        animation: prefersReducedMotion 
          ? 'none' 
          : 'fadeIn 0.3s ease-in-out'
      }}
    >
      Content
    </div>
  );
}
```

```css
/* Alternative: CSS-only approach */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 3.3 ARIA Labels & Semantic HTML

```html
<!-- ANTES: No accesible -->
<div onclick="goToPage('/features')">Features</div>

<!-- AHORA: Accesible -->
<button onclick="goToPage('/features')">
  <span aria-label="Go to features section">Features</span>
</button>

<!-- ARIA en elementos interactivos -->
<div role="region" aria-label="Featured projects">
  <h2>Featured</h2>
  <div class="project-grid" aria-live="polite">
    <!-- Projects listed -->
  </div>
</div>

<!-- ARIA en forms -->
<input
  type="email"
  placeholder="your@email.com"
  aria-label="Email address"
  aria-required="true"
  aria-invalid="false"
/>

<!-- ARIA en navigation -->
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

<!-- ARIA en modals -->
<dialog aria-labelledby="dialog-title" aria-describedby="dialog-desc">
  <h2 id="dialog-title">Confirm action</h2>
  <p id="dialog-desc">Are you sure?</p>
  <button>Cancel</button>
  <button>Confirm</button>
</dialog>
```

### 3.4 Color Blindness Validation

```css
/* No confiar solo en color */
/* ❌ MALO: Solo color indica estado */
.success { color: green; }
.error { color: red; }

/* ✅ BUENO: Color + icono + patrón */
.success { 
  color: #4CAF50; 
  border-left: 4px solid #4CAF50;
}
.success::before { content: '✓ '; }

.error { 
  color: #F44336; 
  border-left: 4px solid #F44336;
}
.error::before { content: '✕ '; }

/* Validar paletas en Contrast Analyzer */
/* https://www.tpgi.com/color-contrast-checker/ */
```

---

## 📋 Tarea 4: Validación Lighthouse 98+

### 4.1 Audit Automático

```bash
# Instalar lighthouse CLI
npm install -g lighthouse

# Auditar sitio
lighthouse https://tu-sitio.com --view

# Resultado esperado en Phase 2:
# ✅ Performance: 95+
# ✅ Accessibility: 98+
# ✅ Best Practices: 95+
# ✅ SEO: 90+
# ✅ Overall: 94.5 (trending to 98)
```

### 4.2 Métricas Core Web Vitals

```javascript
// src/lib/metrics.ts
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

const metrics = {
  CLS: null,  // Cumulative Layout Shift < 0.05
  FID: null,  // First Input Delay < 100ms
  LCP: null,  // Largest Contentful Paint < 2.5s
  TTL: null,  // Time to First Byte < 0.6s
};

getCLS(result => { metrics.CLS = result.value; });
getFID(result => { metrics.FID = result.value; });
getLCP(result => { metrics.LCP = result.value; });
getTTFB(result => { metrics.TTL = result.value; });

// Log a analytics
console.log('Web Vitals:', metrics);
```

### 4.3 Performance Checklist

- [ ] Images optimized (WebP, lazy-load)
- [ ] JavaScript minified & code-split
- [ ] CSS tree-shaken (remove unused)
- [ ] Fonts: subset + display:swap
- [ ] Third-party scripts deferred
- [ ] Preconnect to CDNs
- [ ] Cache headers configured (3600s+)
- [ ] Gzip compression enabled

---

## ✅ Phase 2 Checklist

### Tipografía & Espaciado
- [ ] Implementar clamp() para responsive typography
- [ ] Progressive spacing (section gaps)
- [ ] Contrast ratios validados (7:1 minimum AAA)
- [ ] h1 scale: 2.5rem mobile → 5rem desktop

### Micro-interacciones
- [ ] Button states (default, hover, active, focus, disabled)
- [ ] Card elevation (box-shadow on hover)
- [ ] Skeleton loaders (shimmer animation)
- [ ] Toast notifications (slide + fade)

### Accesibilidad
- [ ] Focus rings styled (4px, visible)
- [ ] Skip links implementados
- [ ] Reduced motion detection active
- [ ] ARIA labels completos
- [ ] Semantic HTML (button, nav, section, etc.)
- [ ] Color blindness: icono + color (no solo color)

### Validación Quality
- [ ] Lighthouse 95+ (all categories)
- [ ] Core Web Vitals Excellent (CLS, FID, LCP, TTL)
- [ ] WCAG AAA accessibility
- [ ] Cross-browser testing (Chrome, Safari, Firefox)
- [ ] Mobile viewport tested (320px - 2560px)

---

## 🚀 Siguientes Pasos (Phase 3)

Una vez completado Phase 2:

**Fase 3: Video Integration (Semana 5-6)**
- [ ] Producir/compilar videos (hero, features, testimonios)
- [ ] Implementar canvas frame rendering
- [ ] Integrar Lenis + GSAP choreography
- [ ] Optimizar video delivery (CDN, lazy-load, compression)

---

**Status**: 🟡 Ready to Implement  
**Estimated Duration**: 2-3 semanas  
**Dependencies**: Phase 1 ✅  
**Owner**: UX Enhancement Team

