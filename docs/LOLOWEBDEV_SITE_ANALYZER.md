# LOLOWEBDEV Site Analyzer & Mockup Generator
**Discovery + 3 ADN Variants + Selection Workflow**

---

## 🎯 Propósito

Analizar un sitio existente (o descripción) y generar **3 mockups completamente diferentes** con distintos "ADN" (estilos, arquitecturas, target audience) para que el usuario **vea opciones concretas** antes de comprometerse con una dirección.

**Flujo**:
```
Usuario comparte sitio/idea
    ↓
[ANÁLISIS] Hacer preguntas de discovery
    ↓
[GENERACIÓN] Crear 3 mockups variantes
    ↓
[SELECCIÓN] Usuario elige ADN favorito + refina
    ↓
[IMPLEMENTACIÓN] Aplicar Phase 1-4 con ese ADN
```

---

## 📋 Workflow: Site Analyzer

### Step 1: Intake & Analysis

**Usuario proporciona**:
```
- URL actual (si existe)
- Descripción del proyecto
- Objetivo principal
- Público target
- Budget/timeline (opcional)
```

**Ejemplo**:
```
"Tengo una agencia de diseño pequeña, portfolio básico en Webflow.
Quiero mostrar casos mejor y que se vea más premium.
Target: marcas de lujo y startups creativas.
Timeline: 2 meses."
```

### Step 2: Discovery Questions

Hacer **preguntas inteligentes** para entender:

```markdown
## Discovery Questions (Contexto)

1. **Aesthetic Preference**
   - "¿Qué sitios te inspiran? (lujo, tech, playful, minimal)"
   - Proporcionar ejemplos visuales de cada estilo

2. **Video Appetite**
   - "¿Quieres hero video cinematográfico o prefer static?"
   - "¿Presupuesto para producción de video?"
   - "¿Videos existentes o empezar desde cero?"

3. **Animation Comfort**
   - "¿Scroll cinematográfico Apple-style o más minimal?"
   - "¿Animaciones en cada interacción o solo lo esencial?"

4. **Content Type**
   - "¿Portfolio heavy, case studies, testimonios, manifesto?"
   - "¿Blog/news o solo portfolio?"

5. **Technical Requirements**
   - "¿CMS needed? (Contentful, Sanity, Strapi)"
   - "¿Ecommerce integration? (Shopify, custom)"
   - "¿API connections? (Calendly, Stripe, etc)"

6. **Performance Needs**
   - "¿Lighthouse 98+ es crítico o 90+ es ok?"
   - "¿Core Web Vitals monitored?"

7. **Mobile Priority**
   - "¿Mobile-first o desktop-first experience?"
   - "¿Tablet optimization importante?"
```

### Step 3: Generate 3 Mockups

Basado en análisis + discovery, generar **3 completamente distintas**:

```
VARIANT A: "Luxury Minimal" (Conservative)
├─ Style: Luxury Modern (Cormorant serif + dorado)
├─ Video: Static hero + subtle motion
├─ Animation: Rich but elegant
├─ Sections: Portfolio + Testimonios + Contact
└─ ADN: Premium, refined, traditional

VARIANT B: "Tech Forward" (Progressive)
├─ Style: Tech Premium (Plus Jakarta Sans + vibrant)
├─ Video: Hero video 360°
├─ Animation: 11 animation types, magnetic snap
├─ Sections: Reel + Archive + Process + Stats
└─ ADN: Modern, dynamic, data-driven

VARIANT C: "Brutalist Bold" (Provocative)
├─ Style: Brutalism (Outfit bold + black/white)
├─ Video: Heavy motion graphics
├─ Animation: Jump cuts, clip-path reveals
├─ Sections: Manifesto + Projects + Lab + Community
└─ ADN: Bold, provocative, editorial
```

---

## 🎨 Mockup Generation Prompts

### Para Variant A: Luxury Minimal

```prompt
Eres un diseñador de lujo. Crea un mockup elegante y refinado para [CLIENTE].

RESTRICCIONES:
- Tipografía: Cormorant Garamond (serif) para headings, Inter para body
- Colores: Dorado (#D4A574) + Negro (#1a1a1a) + Crema (#F5F3F0)
- Video: Static hero (no video)
- Animaciones: Subtle fade-in, smooth scroll, 200ms transitions
- Secciones: 5-6 max (no overwhelm)
- Typography: Large, generous spacing, breathing room

ESTRUCTURA:
1. Fixed navbar (simple, minimal)
2. Hero (static image + elegant tagline)
3. Featured Work (2-3 projects, large imagery)
4. Philosophy/Manifesto (text-heavy, serif)
5. Testimonios (3 cliente quotes)
6. Contact (email form, minimal)

OUTPUT: HTML mockup con Tailwind, NO videos, transiciones suaves
```

### Para Variant B: Tech Forward

```prompt
Eres un creative technologist. Crea un mockup futurista y dinámico para [CLIENTE].

RESTRICCIONES:
- Tipografía: Plus Jakarta Sans (bold) + Inter (body)
- Colores: Vibrant gradient + neon accents
- Video: Hero video 360° product demo
- Animaciones: 11 animation types, magnetic snap-stop, 1.8-2.2 frame speed
- Secciones: 7-8 (content-heavy)
- Canvas: Frame rendering con scroll binding

ESTRUCTURA:
1. Navbar pill (glassmorphism on scroll)
2. Hero Video (canvas frame rendering)
3. Feature Tabs (video demos per feature)
4. Testimonios (video clips 5-8s)
5. Stats (counter animations)
6. Pricing/Plans
7. CTA (persist to bottom)
8. Newsletter signup

OUTPUT: HTML mockup con GSAP + Lenis, videos, full Premium Scroll Checklist
```

### Para Variant C: Brutalist Bold

```prompt
Eres un diseñador editorial. Crea un mockup provocador y boldly diseñado para [CLIENTE].

RESTRICCIONES:
- Tipografía: Outfit Bold (monospace feel) massive (12rem+ hero)
- Colores: Black (#000) + White (#FFF) + 1 saturated accent color
- Video: Motion graphics heavy, jump cuts
- Animaciones: Clip-path reveals, rotate-in, no smooth fades
- Secciones: 6-7 (editorial layout)
- Grid: Asymmetric, breaking rules

ESTRUCTURA:
1. Hero (massive typography, no navbar)
2. Manifesto (bold statements, breakout layout)
3. Project Showcase (full-width, asymmetric grid)
4. Process (timeline, motion graphics)
5. Lab/Experiments (cutting edge work)
6. Community/Talks (events, speaking gigs)
7. Contact (minimal, just email)

OUTPUT: HTML mockup con motion graphics, clip-path, experimental layout
```

---

## 🎯 Selection & Refinement

### After User Selects Variant

```
User: "Me gusta Variant B pero más minimalista, 
        sin tantas animaciones, y no necesito videos."

Claude:
1. Entiendo - combinamos "Tech Forward" visual con "Luxury Minimal" restraint
2. Creo Variant B.2 Hybrid con:
   ├─ Tech Premium colors (gradiente suave)
   ├─ Fewer animations (fade-up, slide, counter only)
   ├─ No videos (static hero con imagen)
   ├─ 5 secciones (no 8)
   ├─ Navbar pill (keep tech feel)
   └─ Canvas rendering (pero para stats, no hero)

3. Pregunto:
   ├─ "¿Esta mezcla así está mejor?"
   ├─ "¿Qué te falta de Variant A que quieras?"
   ├─ "¿Qué de Variant C te atrae?"
   └─ "¿Listo para pasar a Phase 1?"
```

---

## 🛠️ Integration con Phases 1-4

Una vez seleccionado ADN, el workflow es:

```
ADN Seleccionado: "Tech Minimal Hybrid"
    ↓
[PHASE 1] Foundation
├─ Design tokens con Tech Premium palette
├─ Seleccionar dominio (SaaS B2B, Agency, etc)
├─ Lighthouse audit actual
└─ Create design-tokens.ts

[PHASE 2] UX Enhancements  
├─ Implementar button states (4 estados)
├─ Micro-interacciones (solo las necesarias)
├─ Focus rings + ARIA labels
└─ Validar WCAG AAA

[PHASE 3] Video Integration
├─ Decidir: video o no? (ya determinado en mockup)
├─ Si video: setup canvas rendering
├─ Si no: optimizar lazy-load images
└─ Lenis + GSAP setup (mínimo en este caso)

[PHASE 4] Testing + Deploy
├─ Jest tests
├─ Lighthouse validation
├─ A/B testing si aplica
└─ Production deployment
```

---

## 📊 Mockup Comparison Matrix

```
                    VARIANT A          VARIANT B          VARIANT C
                 Luxury Minimal     Tech Forward      Brutalist Bold
────────────────────────────────────────────────────────────────────
Style             Luxury Modern      Tech Premium        Brutalism
Colors            Gold/Black/Cream   Gradient/Neon      Black/White/1 Accent
Typography        Serif Large        Sans Bold          Sans Massive
Videos            ❌ Static          ✅ Hero Video      ✅ Motion Graphics
Animations        Subtle (3-4)       Premium (11)       Experimental (5-6)
Canvas            ❌ No             ✅ Yes             ✅ Yes
Sections          5-6               7-8                6-7
Navbar            Simple Fixed       Pill Transform     None (hero full)
Complexity        Low               High               Medium
Target            Luxury Brands     Tech Startups      Agencies/Creative
Setup Time        1-2 weeks         3-4 weeks          2-3 weeks
```

---

## 🎨 HTML Mockup Template Structure

Cada mockup tiene esta estructura en HTML:

```html
<!-- VARIANT A: Luxury Minimal -->
<html>
  <head>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital@0;1&family=Inter:wght@400;600&display=swap" rel="stylesheet">
    <style>
      :root {
        --color-primary: #D4A574;
        --color-accent: #1a1a1a;
        --color-surface: #F5F3F0;
        --font-serif: 'Cormorant Garamond', serif;
        --font-body: 'Inter', sans-serif;
        --transition: 200ms cubic-bezier(0.4, 0, 0.2, 1);
      }
      /* Minimal CSS, elegant styling */
    </style>
  </head>
  <body>
    <!-- 1. Navbar: Simple, fixed -->
    <!-- 2. Hero: Static image, large serif heading -->
    <!-- 3. Featured Work: 2-3 projects, 50/50 image-text -->
    <!-- 4. Philosophy: Serif italic text, spacious -->
    <!-- 5. Testimonios: 3 quotes, author names -->
    <!-- 6. Contact: Email form, minimal -->
  </body>
</html>
```

Cada mockup es **fully functional HTML + CSS** (sin JavaScript para mockup puro).

---

## 📱 Mockup Delivery Format

Cada mockup se entrega como:

```
VARIANT_A_LUXURY_MINIMAL/
├── index.html (fully styled)
├── assets/
│   ├── hero.jpg (placeholder)
│   ├── project-1.jpg
│   └── logo.svg
└── README.md (descripción ADN + notas)

VARIANT_B_TECH_FORWARD/
├── index.html (GSAP preview, no video)
├── assets/
│   ├── hero-video-preview.jpg
│   ├── frame-*.webp (samples)
│   └── logo.svg
└── README.md (arquitectura + next steps)

VARIANT_C_BRUTALIST_BOLD/
├── index.html (motion graphics mockup)
├── assets/
│   ├── graphic-1.svg
│   ├── texture.png
│   └── logo.svg
└── README.md (bold direction + rationale)
```

---

## 🎯 Flujo Completo: Ejemplo Real

```
USER: "Tengo una agencia de 5 personas, servicios: branding, web, video.
       Actualmente en Webflow. Portfolio mediocre, quiero mejorar."

CLAUDE (Site Analyzer):
"Perfecto. Voy a hacer algunas preguntas para entender mejor tu ADN."

PREGUNTAS:
1. ¿Qué agencias te inspiran visualmente? (muestra 5 links)
2. ¿Prefieres mostrar casos cerrados o también process/behind-the-scenes?
3. ¿Video es crítico para ti o es bonus?
4. ¿Animaciones cinematográficas o más restrained?
5. ¿Qué es tu ideal: convertir leads, mostrar work, posicionarse como experto?

USER: "Agencias como [links]. Casos cerrados. Video sí pero no hero video. 
       Animations moderate, no overwhelming. Convertir leads (contactos)."

CLAUDE:
"Entendido. Te voy a crear 3 mockups variantes:

A) LUXURY MINIMALIST → Premium pero tranquilo, foco en work quality
B) TECH FORWARD → Video showcase, more dynamic, feature-rich
C) BRUTALIST BOLD → Provocador, mostrás agency point of view (opinion)

Creo los 3 mockups, los ejecuto como HTML funcional, 
y elegís cuál ADN resuena más. Luego refinamos."

[GENERA 3 MOCKUPS en HTML]

"Aquí están. ¿Cuál te atrae más? ¿Querés mezclar elementos de varios?"

USER: "Me encanta A pero con los colores vibrantes de B. 
       Y el grid asimétrico de C."

CLAUDE:
"Perfecto. Creo VARIANT A.5 HYBRID:
- Luxury Minimal structure (5 sections, refined)
- Tech colors (gradient suave, neon accent)
- Asymmetric grid en featured work
- Moderate animations (A level + counter animations)
- Video en features section (no hero)

¿Este mix está mejor?
Si sí → Pasamos a Phase 1 con este ADN.
Si no → Sigo refinando."
```

---

## 🚀 Integration en lolowebdev Skill

El Site Analyzer se invoca así:

```
User: "Tengo un sitio que quiero mejorar, 
       analiza y crea mockups para que vea opciones"

Claude (con Site Analyzer):

1. "Voy a analizar tu sitio y crear 3 variantes de ADN para ti.
    ¿Puedo ver el URL actual? O me describes qué tienes?"

2. [Make Discovery Questions]

3. [Generate 3 Mockups + HTML]

4. "Aquí están 3 direcciones completamente distintas.
    Elige la que más resuena y refinamos juntos."

5. [Refinement Loop]

6. [Apply Phases 1-4 con ADN seleccionado]
```

---

## 📋 Site Analyzer Checklist

- [ ] Intake form clear (URL or description)
- [ ] Discovery questions comprehensive (7 questions)
- [ ] 3 Mockup variants diverse (luxury, tech, brutalist)
- [ ] Mockups functional HTML (not just screenshots)
- [ ] Selection UI intuitive (A vs B vs C comparison)
- [ ] Refinement workflow clear (hybrid options)
- [ ] Integration seamless (flows to Phase 1)
- [ ] Documentation complete (this file)

---

## 🎓 Benefits

✅ **Visual Before Commitment**: User ve opciones concretas antes de phase 1  
✅ **Discovery Efficiency**: Questions are guided, not open-ended  
✅ **ADN Clarity**: User sabe exactamente el estilo que quiere  
✅ **Hybrid Flexibility**: Puede mezclar lo mejor de cada variante  
✅ **Time Savings**: Evita pivots a mitad de Phase 1-4  
✅ **Confidence**: Cliente ve mockup antes de inversión real  

---

**Status**: 🟡 Ready to Implement as Site Analyzer Feature  
**Integration**: Seamless with Phase 1-4  
**Value Add**: Discovery + ADN Selection before full implementation  

