# LOLOWEBDEV - Guía Rápida de Uso

## Para Activar el Skill

Simplemente escribe cualquiera de estas variantes en una conversación:

```
/lolowebdev
creá una landing cinematográfica para mi SaaS
necesito un sitio web apple-style premium
genera un instrumento digital
landing lolowebdev para mi marca
web pixel-perfect con scroll suave
```

---

## Flujo Básico (3 Pasos)

### 1️⃣ Dispara el Skill
```
Usuario: "Necesito una landing para Synthia, una plataforma de IA generativa"
```

### 2️⃣ Responde las Preguntas Rápidas
```
Sistema: "¿Cuál es tu CTA? ¿Qué identidad visual? ¿Qué secciones?"

Usuario proporciona:
- CTA: "Reserva demo"
- Estética: "Tech Premium"
- Secciones: Hero, Features, Pricing, Footer
```

### 3️⃣ Obtén el Código
```
Sistema genera:
✅ Proyecto React 19 + GSAP 3 completo
✅ Estructura de componentes
✅ Hooks reutilizables
✅ Design system integrado
✅ Animaciones Apple-style

$ npm install && npm run dev
→ http://localhost:3000
```

---

## Después de Generar

### Personalización Común

```
"Cambia el color primary a azul"
→ Sistema actualiza tokens y regenera con nuevo color

"Más dramático el scroll del archivo"
→ Aumenta factor parallax, timing de animaciones

"Añádile un formulario de email en el hero"
→ Integra form con validación y envío

"Toggle dark mode"
→ Añade Context + localStorage + CSS variables

"Integra Calendly al botón CTA"
→ Embeds modal de Calendly
```

---

## Casos Típicos (Copy-Paste)

### SaaS B2B
```
/lolowebdev

Brand: Synthia (IA para diseño)
Audience: CTOs de agencias
CTA: Reserva demo
Aesthetic: Tech Premium
Sections: Hero, Features (terminal + calendar), Pricing, Testimonios
```

### Agencia Creativa
```
/lolowebdev

Brand: Studio Creativo XYZ
Audience: Brands que buscan identidad
CTA: Enviar brief
Aesthetic: Lujo Moderno
Sections: Hero, Archivo (proyectos), Manifiesto, Testimonios
```

### Startup Pre-Seed
```
/lolowebdev

Brand: StartupName
Audience: Founders, inversores early
CTA: Únete a beta
Aesthetic: Minimalismo Oscuro
Sections: Hero, Features, Early Access Form, Roadmap
```

---

## Archivos Incluidos en la Generación

```
lolowebdev-synthia/
├── src/
│   ├── app/
│   │   ├── layout.tsx (meta tags, providers)
│   │   └── page.tsx (página principal)
│   ├── components/
│   │   ├── Hero.tsx
│   │   ├── Features/
│   │   │   ├── StackedCards.tsx
│   │   │   ├── Terminal.tsx
│   │   │   └── Calendar.tsx
│   │   ├── Manifest.tsx
│   │   ├── Archive.tsx
│   │   ├── Testimonials.tsx
│   │   ├── Pricing.tsx
│   │   ├── Navbar.tsx
│   │   └── Footer.tsx
│   ├── hooks/
│   │   ├── useScrollTrigger.ts
│   │   ├── useParallax.ts
│   │   └── useStackedCards.ts
│   ├── styles/
│   │   └── globals.css (design tokens + animations)
│   └── lib/
│       └── animations.ts (GSAP helpers)
├── public/
│   └── images/ (optimizadas)
├── package.json
├── next.config.js
├── tailwind.config.js
└── README.md
```

---

## Palabras Clave Avanzadas (Segunda Pasada)

Una vez generado, puedes pedir refinamientos específicos:

```
Palabras clave de ANIMACIÓN:
"Más rápido el scroll del hero"
"Parallax dramático en el manifiesto"
"Baraja de cards más lenta"
"Efecto 3D en el archivo"
"Desvanecimiento suave entre secciones"

Palabras clave de FUNCIONALIDAD:
"Integra formulario de email"
"Añade newsletter subscription"
"Conecta a una base de datos"
"Tracking de eventos con analytics"
"WhatsApp chat flotante"

Palabras clave de DISEÑO:
"Cambiar paleta de colores"
"Ajustar tipografía"
"Dark mode automático"
"Mobile-first refinement"
"Contrast WCAG AA check"

Palabras clave de COMPONENTES:
"Añade sección FAQ"
"Integra carousel de testimonios"
"Heatmap interactivo"
"Timeline de roadmap"
"Comparador de planes"
```

---

## Checklist Pre-Deploy

- [ ] Imágenes optimizadas (WebP, lazy-load)
- [ ] Copy actualizado (sin placeholders)
- [ ] CTA funcional (formulario, API, webhook)
- [ ] Mobile responsive (testear en xs, sm, md)
- [ ] Lighthouse score > 90
- [ ] WCAG AA accesible (contrast, ARIA, keyboard nav)
- [ ] Analytics integrado (Google, Plausible)
- [ ] 404 page personalizada
- [ ] Robots.txt y sitemap.xml
- [ ] SSL en dominio propio

---

## Deploy en 2 Minutos (Vercel)

```bash
# 1. Conecta tu repo GitHub
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/tu-usuario/lolowebdev-synthia

# 2. Sube a GitHub
git push -u origin main

# 3. Conecta a Vercel
# → Abre vercel.com/dashboard
# → "Add New..." → "Project"
# → Selecciona tu repo
# → Deploy automático en 30 segundos

# O desde CLI:
npx vercel --prod
```

---

## Soporte & Refinamientos

En cualquier momento puedes pedir cambios:

```
"El titulo del hero es muy pequeño en mobile"
→ Regenera con responsive typography ajustado

"Quiero un CTA secundario en el footer"
→ Añade botón flotante + scroll-to-section

"Los colores no representan mi marca"
→ Cambia tokens y regenera todo

"Integra con mi CRM"
→ Configura webhook a tu plataforma

"Más audaz, menos minimalista"
→ Aumenta saturación, añade efectos visuales
```

---

## Tips de Productividad

1. **Guarda tu briefing**
   ```
   brand.json
   {
     "name": "Synthia",
     "cta": "Reserva demo",
     "aesthetic": "tech-premium",
     "tone": "Confiable, inteligente, productivo"
   }
   ```

2. **Versioná cambios** (Git workflow)
   ```bash
   git checkout -b feature/dark-mode
   # ... cambios ...
   git commit -m "feat: añadir dark mode toggle"
   git push origin feature/dark-mode
   ```

3. **Testea en múltiples dispositivos**
   - Chrome DevTools (xs, sm, md, lg, xl)
   - iPhone/Android real
   - Tablets

4. **Monitorea performance**
   - PageSpeed Insights
   - Lighthouse
   - Web Vitals extension

---

## Limitaciones & Notas

⚠️ **No es un CMS** → Edita copy en componentes React, usa MDX si necesitas
⚠️ **No es headless** → Para completa flexibilidad, integra Contentful/Strapi
⚠️ **Animaciones en scroll** → Desactívate en prefers-reduced-motion (automático)
⚠️ **Imágenes pesadas** → Usa Unsplash, Pexels, o tu CDN (Cloudinary)

✅ **Es production-ready** → Deploy inmediato sin refactoring
✅ **Es escalable** → Fácil agregar secciones, componentes, funcionalidad
✅ **Es accesible** → WCAG AA compilado
✅ **Es rápido** → Lighthouse 95+ en Core Web Vitals

---

**Versión**: 1.0 Quick Start
**Última actualización**: 2026-05-20