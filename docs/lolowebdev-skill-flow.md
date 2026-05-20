# LOLOWEBDEV Skill - Flujo de Ejecución

## Descripción Corta para Sistema de Skills

```
Genera instrumentos digitales de alta fidelidad: landing pages cinematográficas 
con scroll Apple-style, GSAP 3, design system robusto y cero placeholders. 
Tech Premium + Lujo Moderno. Pixel-perfect, interacciones magnéticas, copy real.
```

## Triggers (Palabras Clave para Activar)

```
"creá una landing", "genera un sitio web", "landing cinematográfica"
"web lolowebdev", "instrumento digital", "apple-style scroll"
"landing premium", "web pixel-perfect", "lolowebdev"
"crea un sitio", "página de ventas moderna"
```

---

## Flujo de Ejecución del Skill

### FASE 1: INTAKE (Preguntas de Recopilación)

Cuando el usuario dispara el skill, hacer preguntas **secuenciales y tácitas**:

```
USER: "Necesito una landing para mi startup"
↓
SKILL: "Perfecto. Vamos a armarla juntos. Dime:"

1️⃣ "¿Cuál es el nombre/sector de tu marca?"
   (Ej: "Synthia - IA generativa para diseño", "TechFlow - automatización")

2️⃣ "¿A quién vendés en máximo una frase?"
   (Ej: "Agencias que necesitan workflows 10x más rápidos")

3️⃣ "¿Cuál es tu CTA principal?"
   (Ej: "Reserva demo", "Únete a beta", "Descargar whitepaper")

4️⃣ "¿Cuál identidad visual te resuena?"
   - ⭐ Tech Premium (verde musgo + arcilla)
   - 🌙 Lujo Moderno (dorado + negro)
   - 🔮 Minimalismo Oscuro (neon + blanco)
   - 🌿 Orgánico Futuro (crema + verde agua)

5️⃣ "¿Qué secciones son críticas?"
   (Multi-select)
   - ✓ Hero + CTA
   - ✓ Features (tarjetas, telemetría, calendario)
   - ✓ Manifiesto (diferencial)
   - ✓ Archivo/Portfolio
   - ✓ Testimonios
   - ✓ Pricing
   - ✓ FAQ
   - ✓ Footer

6️⃣ "¿Quién es tu audience principal?"
   (Ej: "CTOs de agencias", "Founders pre-seed", "Diseñadores independientes")

7️⃣ "¿Tienes imágenes/assets?"
   - Sí (URLs de Unsplash / propias)
   - No (usamos stock curado)
```

---

### FASE 2: SÍNTESIS

Con los datos, construir un **"briefing técnico"** que alimenta el generador:

```javascript
{
  brand: {
    name: "Synthia",
    tagline: "IA generativa para diseño 10x más rápida",
    audience: "CTOs de agencias digitales",
    cta: "Reserva demo",
    aesthetic: "tech-premium"
  },
  sections: {
    hero: true,
    features: ["stacked-cards", "terminal", "calendar"],
    manifest: true,
    archive: true,
    testimonials: true,
    pricing: true,
    faq: false,
    footer: true
  },
  assets: {
    heroImage: "https://images.unsplash.com/...",
    colors: { primary: "#2E4036", accent: "#CC5833", ... }
  },
  tone: "Tech premium, sin hype, enfoque en resultados"
}
```

---

### FASE 3: GENERACIÓN

Con el briefing listo, disparar el **prompt maestro mejorado**:

```markdown
ROL Y OBJETIVO
[LOLOWEBDEV - Senior Creative Technologist]

PARÁMETROS
- Marca: ${brand.name}
- Proposición: ${brand.tagline}
- Audience: ${brand.audience}
- CTA: ${brand.cta}
- Estética: ${brand.aesthetic}

SECCIONES A INCLUIR
${sections.map(s => `✓ ${s}`).join('\n')}

[INSERTAR AQUÍ EL PROMPT BASE COMPLETO]

INSTRUCCIONES FINALES
1. Genera el código React 19 + GSAP 3 + Tailwind completo
2. Usa URLs reales de imágenes (Unsplash o similares)
3. Cero placeholders, copy coherente con ${brand.name}
4. Animaciones Apple-style smooth scroll
5. Design system: colores exactos [colores del briefing]

ENTREGA
- Código listo para correr (npm install && npm run dev)
- Componentes bien organizados
- Hooks reutilizables
- Sin warnings en consola
```

---

### FASE 4: ENTREGA

El skill ejecuta y devuelve:

```
✅ Código completo en carpeta /lolowebdev-${brand.name}/
   ├── src/components/  (todos los componentes)
   ├── src/hooks/       (useScrollTrigger, useParallax, etc.)
   ├── src/styles/      (globals.css con design tokens)
   ├── package.json     (dependencies listas)
   ├── next.config.js   (optimizaciones)
   └── README.md        (guía de extensión)

✅ Instrucciones inmediatas:
   $ cd lolowebdev-${brand.name}
   $ npm install
   $ npm run dev
   → http://localhost:3000

✅ Preview link (si está disponible)
```

---

## Opciones Avanzadas (Segunda Pasada)

Si el usuario pide refinamiento:

```
USER: "Quiero que el Feature 1 sea más dramático"
SKILL: "Cambiamos el timing de la baraja (2s → 3.5s), aumentamos blur (8px → 25px)
       y añadimos un scale drop más acentuado (0.95 → 0.85). ¿También color?"

USER: "Añádile un formulario de early access al hero"
SKILL: "Integro formulario flotante (glass-morphism), envía a Supabase,
       toast de confirmación con micro-animation. ¿Qué campos?"
```

---

## Prompt Maestro Inyectado

[El contenido completo de `/lolowebdev-base.md` va aquí, personalizado con datos del usuario]

---

## Variables de Personalización (Plug & Play)

```javascript
// COLORS
const THEME = {
  'tech-premium': {
    primary: '#2E4036',
    accent: '#CC5833',
    bg: '#F2F0E9',
    dark: '#1A1A1A'
  },
  'lujo-moderno': {
    primary: '#0F0F0F',
    accent: '#D4AF37',
    bg: '#FAFAF8',
    dark: '#1B2D5E'
  },
  // ... más temas
}

// COPY
const COPY = {
  cta: "${brand.cta}",
  tagline: "${brand.tagline}",
  tone: "${brand.tone}"
}

// COMPONENTES
const SECTIONS = {
  hero: true,
  features: ['stacked-cards', 'terminal', 'calendar'],
  manifest: true,
  // ... etc
}
```

---

## Casos de Uso Típicos

### 1. SaaS B2B (CTOs)
```
Estética: Tech Premium
Secciones: Hero, Features (terminal), Manifiesto, Pricing, Testimonios
Tone: Credibilidad, datos, automatización
```

### 2. Lujo / Boutique Digital
```
Estética: Lujo Moderno
Secciones: Hero, Archivo (porfolio), Manifiesto, Testimonios, Footer
Tone: Exclusividad, artesanía, detalle
```

### 3. Startup Pre-Seed
```
Estética: Minimalismo Oscuro + Neon
Secciones: Hero, Features, Early Access, Roadmap, Footer
Tone: Urgencia, comunidad, visión futura
```

### 4. Agencia Creativa
```
Estética: Orgánico Futuro
Secciones: Hero, Archivo (proyectos), Manifiesto, Testimonios, Contacto
Tone: Creatividad, resultados, partnership
```

---

## Palabras Clave Avanzadas

El skill también responde a refinerencias específicas:

```
"Quiero que el scroll del archivo sea 3D"
→ Añade perspectiva CSS + rotateX() en GSAP

"Toggle dark mode"
→ Implementa CSS custom properties + localStorage

"Integra calendly al CTA"
→ Embeds Calendly widget en modal overlay

"Añadí un contador de usuarios en vivo"
→ useEffect con fetch a API (simula con números)

"Quiero que el precio oscile según demanda"
→ Animación GSAP que reacciona a scroll, click, etc.

"Hazlo accesible WCAG AA"
→ Revisa contraste, ARIA, navegación por teclado, focus rings
```

---

## Checklist Post-Generación (Usuario)

- [ ] Cambiar imágenes por las tuyas (Unsplash → tu CDN)
- [ ] Ajustar copy/tipografía según voz de marca
- [ ] Configurar CTA principal (formulario, API, redirect)
- [ ] Testear en mobile (xs, sm, md, lg)
- [ ] Verificar Core Web Vitals (Lighthouse)
- [ ] Revisar accesibilidad (keyboard nav, screen reader)
- [ ] Deploy a producción (Vercel, Netlify)
- [ ] Analytics: Plausible, Fathom, o Google Analytics

---

## Ventajas del Skill

✅ **Modular**: Puedes pedir cambios progresivos
✅ **Pixel-Perfect**: Basado en design system robusto
✅ **Zero Placeholders**: Copy real desde el inicio
✅ **Apple-Style**: Scroll cinematográfico probado
✅ **Producción-Ready**: Listo para Deploy sin refactoring
✅ **Iterable**: Cada versión es mejor que la anterior
✅ **No Genérico**: Cada proyecto tiene identidad propia

---

**Versión del Skill**: 1.0 Modular
**Última actualización**: 2026-05-20
**Mantenedor**: lolowebdev System