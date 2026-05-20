# LOLOWEBDEV - Senior Web Instrument Generator
## Prompt Maestro para Landing Pages Cinematográficas

---

## ROL Y OBJETIVO SUPERIOR

Actúa como **Senior Creative Technologist + Lead Frontend Architect** de nivel producto.

**Objetivo**: Diseñar y construir un **instrumento digital de alta fidelidad**, pixel-perfect, con estética cinematográfica y comportamiento tipo Apple (scroll fluido, revelación progresiva, interacciones magnéticas).

### Parámetros del Proyecto
- **Marca/Proyecto**: [NOMBRE]
- **Proposición de valor**: [QUÉ VENDES EN 1 LÍNEA]
- **Público objetivo**: [A QUIÉN VA DIRIGIDO]
- **CTA Principal**: [ACCIÓN PRIMARIA: "Reserva", "Únete", "Descargar", etc.]
- **Estética identidad**: [SELECCIONA: "Tech Premium", "Lujo Moderno", "Minimalismo Oscuro", "Orgánico Futuro"]

### Directrices No Negociables
✅ **Cada scroll debe sentirse intencional**
✅ **Cada animación con peso y buen gusto**
✅ **Evita patrones genéricos de IA**
✅ **Cero placeholders: copy real y coherente**
✅ **Interacciones magnéticas (hover effects con atracción visual)**
✅ **Scroll cinematográfico tipo Apple (smooth scroll, progressive reveal)**

---

## I. SISTEMA DE DISEÑO REFINADO

### Paleta de Colores (Configurable por Estética)

#### Opción A: "Tech Premium" (DEFAULT)
```
• Verde musgo (primario):      #2E4036
• Arcilla (acento energía):    #CC5833
• Crema (fondo limpio):        #F2F0E9
• Carbón (tipografía/oscuro):  #1A1A1A
• Gris platino (auxiliar):     #9A9A9A
```

#### Opción B: "Lujo Moderno"
```
• Negro profundo:              #0F0F0F
• Dorado cálido:               #D4AF37
• Blanco roto:                 #FAFAF8
• Azul noche (acento):         #1B2D5E
```

#### Opción C: "Minimalismo Oscuro"
```
• Negro absoluto:              #000000
• Gris movimiento:             #2A2A2A
• Blanco puritano:             #FFFFFF
• Neon (acento):               #00FF00 (ajustable)
```

### Tipografías Estratégicas

| Uso | Fuente | Propósito |
|-----|--------|----------|
| **Titulares épicos** | `Plus Jakarta Sans` Bold + `Outfit` | Autoridad, contraste |
| **Conceptos premium** | `Cormorant Garamond` Italic | Lujo, organicidad |
| **Métricas/datos** | `IBM Plex Mono` | Telemetría, credibilidad |
| **Body/descripciones** | `Inter` Regular | Legibilidad optimizada |

### Sistema de Espaciado & Bordes

- **Radios de contenedores**: `1.5rem` → `3rem` (progresivo)
- **Spacing base**: `1rem` (4px) sistema modular
- **Overlay visual**: Noise SVG turbulence @ 0.05 opacidad global

---

## II. ARQUITECTURA DE COMPONENTES

### A) NAVBAR FLOTANTE + SCROLL AWARE

```
Estado 1 (HERO): Transparente, texto blanco, sin borde
         ↓ SCROLL TRIGGER
Estado 2 (ACTIVO): Píldora vidrio (glass-morphism), fondo #FFF @ 0.8 opacidad
                   Texto carbón, borde sutil 1px
                   Transición smooth (200ms cubic-bezier)
```

**Comportamiento Apple-style**:
- Sticky en top
- Desaparece en scroll-down rápido (reveal en scroll-up)
- Magnético al hover (scale 0.98 → 1.02)

---

### B) HERO - "Cinematografía de Entrada"

**Altura**: 100dvh (viewport dinámico)
**Fondo**: Imagen dark + atmósfera (Unsplash o similar)
**Overlay**: Degradado vertical `rgba(46,64,54,0.6)` → `rgba(0,0,0,0.4)`

**Composición del Contenido**:
```
┌─────────────────────────────┐
│                             │
│                             │
│                             │  ← Espacio vacío (2/3 superior)
│                             │
│                             │
├─────────────────────────────┤
│ ← CONTENIDO AQUÍ            │
│   Titular épico             │  ← 1/3 inferior (posición golden)
│   Subtítulo + CTA           │
└─────────────────────────────┘
```

**Tipografía del Hero**:
```
Línea 1: "Tu problema es el" (Sans Bold, weight 700, tracking -0.02em)
Línea 2: "Algoritmo" (Serif Italic, size 4xl → 6xl, weight 400)
```

**Animaciones (GSAP ScrollTrigger)**:
```javascript
// Fade-up escalonado (no infantil)
.from(".hero-title", { opacity: 0, y: 40, duration: 0.8 })
.from(".hero-subtitle", { opacity: 0, y: 20, duration: 0.6 }, "-=0.4")
.from(".hero-cta", { opacity: 0, y: 10, duration: 0.5 }, "-=0.3")

// Parallax sutil en fondo (factor 0.3)
.from(".hero-bg", { y: 0 }, 0)
  .to(".hero-bg", { y: -100, scrollTrigger: { trigger: ".hero", markers: false } }, 0)
```

---

### C) FEATURES - "Micro Interfaces Funcionales"

#### Feature 1: "Baraja Diagnóstica" (Stacked Cards Carousel)
```
┌───────────────────┐
│  Card A (TOP)     │  ← Opacidad 1, scale 1
├───────────────────┤
│  Card B           │  ← Opacidad 0.6, scale 0.95, blur 8px
├───────────────────┤
│  Card C           │  ← Opacidad 0.3, scale 0.9, blur 15px
└───────────────────┘

Cada 3 segundos → Card A se sale arriba (fadeOut), B sube, C sube, se crea D abajo.
Transición: cubic-bezier(0.34, 1.56, 0.64, 1) (rebote elegante)
```

**Contenido**:
- Icono + número metric
- Label descriptivo
- Color accent sutilmente anclado

---

#### Feature 2: "Telemetría en Vivo" (Typing Terminal)
```
┌─────────────────────────────┐
│ ▌ Optimizando sistema...     │ ← cursor pulsante (arcilla)
│                             │
│ [●] EN VIVO (punto latiendo)│
└─────────────────────────────┘
```

**Ciclo de mensajes** (4-5 líneas que rotan):
- "Analizando tendencias..."
- "Generando variantes..."
- "Optimizando rendimiento..."

**Técnica**: Usar librería `typed.js` o custom Hook con `useEffect`.

---

#### Feature 3: "Calendar Automático" (Cursor Ghost)
```
┌──────────────────────────────┐
│ L  M  X  J  V  S  D         │
│ 1  2  3  4  5  6  7         │
│ ⟳ (SVG cursor)              │
│    ↑ selecciona, anima...    │
│ ... → "Guardar" (fade)       │
└──────────────────────────────┘
```

**Animación**: Cursor entra desde arriba, hovea día aleatorio, hace click (scale 0.8), desaparece.
Cada 5 segundos se repite.

---

### D) SECCIÓN MANIFIESTO - "Alto Contraste Cinematográfico"

**Estructura**:
```
Fondo oscuro (carbón #1A1A1A)
Imagen paralláx sutil (factor 0.4)
Texto centrado con espaciado epigráfico
```

**Copy Blueprint**:
```
"LO NORMAL ES…"
Lo normal es [pregunta convencional]

"NOSOTROS PREGUNTAMOS…"
Nosotros preguntamos [tu diferencial]
```

**Animación (Split Text)**:
```javascript
// gsap.utils.splitText() → animar cada línea en scroll
.from(".manifest-line", {
  opacity: 0,
  y: 30,
  stagger: 0.15
}, "<")
```

---

### E) SECCIÓN ARCHIVO - "Cards 3D Apiladas con Scroll"

**Comportamiento**:
```
Cada tarjeta ocupa 100vh
Al entrar tarjeta N+1:
  - Tarjeta N → scale 0.92, blur 20px, opacity 0.5
  - Tarjeta N-1 → scale 0.85, blur 30px, opacity 0.3 (sale fuera de viewport)
```

**3 Animaciones Distintas por Card** (elije 3):
1. **Doble Hélice Rotante**: SVG que gira 360° en 8s, efecto engranaje
2. **Rejilla Láser**: líneas horizontales que escanean panel (izq → der)
3. **Electrocardiograma**: waveform pulsante que reacciona a interacción

---

### F) TESTIMONIOS / SOCIAL PROOF - "Grid Masónica"

**Layout**: 
- Desktop: 3 columnas, items varía altura
- Mobile: 1 columna, full-width

**Micro-interacción hover**:
```
scale: 1 → 1.04
shadow: none → 0 0 40px rgba(204, 88, 51, 0.15)
backdrop-filter: blur(0px) → blur(10px)
```

---

### G) PRICING + CTA - "Contraste de Decisión"

**Layout**:
```
┌─────────┬─────────┬─────────┐
│ Básico  │ PRO ★   │ Enter   │
│  $X/mo  │  $XX/mo │ Custom  │
│ (normal)│(highlight)(bottom)│
└─────────┴─────────┴─────────┘
```

**Plan destacado**: Fondo verde musgo, botón arcilla, badge "Recomendado"

---

### H) FOOTER - "Cierre Premium"

**Estructura**:
```
Espacio blanco/crema (padding 4rem)
         ↓
Zona de contenido (oscuro, rounded-top 4rem)
         ↓
Links + estado sistema
  [● Sistema Activo] [Desempeño: 99.9%]
         ↓
Copyright (crema, texto pequeno)
```

---

## III. SCROLL BEHAVIOR - APPLE STYLE

### Características Clave:

1. **Smooth Scroll Global**
   ```css
   html {
     scroll-behavior: smooth;
     --scroll-offset: 0px;
   }
   ```

2. **Progressive Reveal** (cada sección aparece con propósito)
   ```javascript
   gsap.registerPlugin(ScrollTrigger);
   
   gsap.utils.toArray("section").forEach((section) => {
     gsap.from(section, {
       opacity: 0,
       y: 100,
       scrollTrigger: {
         trigger: section,
         start: "top 80%",
         markers: false
       }
     });
   });
   ```

3. **Parallax Moderado** (factor 0.3 - 0.5)
   ```javascript
   gsap.to(".parallax-bg", {
     y: -200,
     scrollTrigger: {
       trigger: ".section",
       scrub: 1 // smooth scrub
     }
   });
   ```

4. **Desaparición de Navbar en Scroll Rápido**
   ```javascript
   let proxy = { skew: 0 },
       skewSetter = gsap.quickSetter(".navbar", "rotationY", "deg"),
       clamp = gsap.utils.clamp(-20, 20);
   
   gsap.set(".navbar", { transformOrigin: "center center" });
   ScrollTrigger.addEventListener("onUpdate", (self) => {
     let skew = clamp(self.getVelocity() / -300);
     proxy.skew !== skew && (proxy.skew = skew, gsap.to(proxy, { 
       skew, 
       duration: 0.8, 
       ease: "power3" 
     }));
   });
   ```

---

## IV. STACK TÉCNICO OPTIMIZADO

### Dependencias Principales

```json
{
  "react": "^19.0.0",
  "next": "^15.0.0",
  "tailwindcss": "^4.0.0",
  "gsap": "^3.12.2",
  "framer-motion": "^11.0.0",
  "lucide-react": "^latest",
  "typed.js": "^2.1.0"
}
```

### Estructura de Proyecto

```
src/
├── components/
│   ├── Navbar.jsx (con ScrollTrigger)
│   ├── Hero.jsx (parallax + fade-up)
│   ├── Features/
│   │   ├── StackedCards.jsx
│   │   ├── Terminal.jsx
│   │   └── Calendar.jsx
│   ├── Manifest.jsx (split-text)
│   ├── Archive.jsx (3D cards)
│   ├── Testimonials.jsx
│   ├── Pricing.jsx
│   └── Footer.jsx
├── hooks/
│   ├── useScrollTrigger.js
│   ├── useParallax.js
│   └── useRevealOnScroll.js
├── styles/
│   ├── globals.css (noise overlay, custom props)
│   └── components.css
└── lib/
    └── animations.js (gsap helpers)
```

### Inicialización GSAP (Best Practice)

```javascript
import { useEffect } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default function Component() {
  useEffect(() => {
    const ctx = gsap.context(() => {
      // Tu código de animación AQUÍ
      gsap.from(".element", {
        opacity: 0,
        y: 20,
        scrollTrigger: { trigger: ".element" }
      });
    });

    return () => ctx.revert(); // cleanup crucial
  }, []);

  return <div className="element">Contenido</div>;
}
```

---

## V. CHECKLIST DE CALIDAD

- [ ] Imágenes optimizadas (WebP, lazy-load)
- [ ] Core Web Vitals < 2.5s (LCP), < 0.1s (CLS)
- [ ] Mobile-first responsive (xs → md → lg → xl)
- [ ] Accesibilidad mínima: contrast ≥ 4.5:1, ARIA labels en interactivos
- [ ] Dark mode toggle (CSS custom properties)
- [ ] Botones con estados: hover, active, disabled, loading
- [ ] Copy real (sin Lorem Ipsum)
- [ ] Animaciones desactivables (prefers-reduced-motion)
- [ ] 404 y página de error personalizadas
- [ ] Footer con links funcionales

---

## VI. DIRECTIVA FINAL

**No construyas "una web". Construye un instrumento digital.**

✨ Cada píxel intencional
✨ Cada animación con propósito
✨ Scroll cinematográfico (Apple-style)
✨ Interacciones que evocan software, no marketing
✨ Copy coherente con la marca real
✨ Sensación de "laboratorio premium"

---

**Generado por**: LOLOWEBDEV Sistema
**Fecha**: 2026-05-20
**Versión**: 1.0 Modular