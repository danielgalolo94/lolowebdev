---
name: LOLOWEBDEV Skill - Sistema de Generación de Landing Pages Cinematográficas
description: Sistema modular para generar sitios web premium con scroll Apple-style, GSAP 3, React 19, design system robusto, cero placeholders
type: project
originSessionId: 45064b6b-9342-40e2-aad0-06479d8bdf8e
---
## Descripción General

**LOLOWEBDEV** es un skill profesional que genera **instrumentos digitales de alta fidelidad** (landing pages cinematográficas) con:

- ✨ **Scroll Apple-style**: smooth, cinematográfico, revelación progresiva
- 🎬 **GSAP 3 + ScrollTrigger**: animaciones de peso y propósito
- 🎨 **Design system robusto**: paletas configurables, tipografía estratégica, tokens
- ⚡ **React 19 + Next.js 15**: Server Components, optimización automática
- 🖼️ **Cero placeholders**: copy real, imágenes Unsplash, estructura profesional

## Documentos de Referencia

| Archivo | Propósito |
|---------|-----------|
| `/home/dgar/.claude/prompts/lolowebdev-base.md` | Prompt maestro mejorado con arquitectura de componentes |
| `/home/dgar/.claude/prompts/lolowebdev-skill-flow.md` | Flujo de ejecución del skill (intake, síntesis, generación, entrega) |
| `/home/dgar/.claude/prompts/lolowebdev-technical-reference.md` | Referencia técnica: componentes, hooks, configuración Next.js |
| `/home/dgar/.claude/prompts/lolowebdev-quick-start.md` | Guía rápida de uso para activar y personalizar |

## Componentes Disponibles

1. **Navbar Flotante** - Scroll-aware, glass-morphism, transición suave
2. **Hero Cinematográfico** - 100vh, parallax, fade-up escalonado
3. **Features**:
   - Stacked Cards Carousel (baraja automática cada 3s)
   - Terminal Typing (telemetría en vivo con cursor pulsante)
   - Calendar Ghost Cursor (cursor automático selecciona días)
4. **Manifest** - Alto contraste, split-text animation, parallax
5. **Archive** - 3 cards 3D apiladas con scroll, 3 animaciones distintas
6. **Testimonios** - Grid masónico, hover magnético
7. **Pricing** - 3 planes, destaque en el medio
8. **Footer** - Rounded-top, estado sistema, links premium

## Opciones de Estética (Configurables)

- **Tech Premium** (default): Verde musgo #2E4036 + Arcilla #CC5833 + Crema #F2F0E9
- **Lujo Moderno**: Negro #0F0F0F + Dorado #D4AF37 + Blanco roto #FAFAF8
- **Minimalismo Oscuro**: Negro absoluto + Gris + Blanco + Neon
- **Orgánico Futuro**: Paleta natural con verdes agua y tonos terracota

## Palabras Clave para Activar el Skill

- `/lolowebdev`
- "creá una landing cinematográfica"
- "web apple-style premium"
- "genera un instrumento digital"
- "landing lolowebdev para mi marca"

## Flujo de Ejecución

1. **Intake**: Preguntas sobre brand, audience, CTA, estética, secciones
2. **Síntesis**: Generación de briefing técnico (JSON)
3. **Generación**: Inyección en prompt maestro + generación de código
4. **Entrega**: Proyecto Next.js completo, listo para `npm install && npm run dev`

## Tech Stack Obligatorio

- React 19
- Next.js 15 (App Router)
- Tailwind CSS 4
- GSAP 3 + ScrollTrigger
- Framer Motion 11
- Lucide React
- Typed.js (para terminal)
- TypeScript

## Post-Generación (Refinamientos Comunes)

El skill responde a palabras clave como:
- "Más dramático el scroll" → Aumenta parallax, timing
- "Cambiar colores" → Regenera con nueva paleta
- "Integra Calendly" → Embeds en modal
- "Dark mode" → CSS variables + Context
- "Formulario de email" → Añade form + integración

## Estructura de Archivos Generados

```
lolowebdev-{brand}/
├── src/components/     (Hero, Features, Manifest, Archive, etc)
├── src/hooks/          (useScrollTrigger, useParallax, useStackedCards)
├── src/styles/         (globals.css con tokens y animaciones)
├── public/images/      (optimizadas para web)
├── package.json        (dependencias listas)
└── next.config.js      (optimizaciones)
```

## Ventajas Principales

✅ **Modular**: Preguntas, síntesis, generación, refinamientos progresivos
✅ **Pixel-Perfect**: Basado en design system probado
✅ **Cero Placeholders**: Copy y estructura reales desde el inicio
✅ **Apple-Style**: Scroll cinematográfico, smooth, revelación progresiva
✅ **Production-Ready**: Listo para Deploy sin refactoring
✅ **No Genérico**: Cada proyecto tiene identidad propia basada en briefing

## Status Actual

- **Versión**: 1.0 Modular (Mayo 2026)
- **Componentes**: 8 principales + variaciones
- **Documentación**: Completa (base, flow, técnica, quick-start)
- **Prompts**: 1 maestro + 1 flujo + referencias técnicas
- **Ready for**: Implementación como skill profesional

## Próximos Pasos (Futuro)

1. Crear interfaz de configuración visual (UI para briefing)
2. Añadir más tipos de estética (Brutalism, Glassmorphism, etc)
3. Integración con CMS (Contentful, Strapi)
4. Template library (variations de componentes)
5. Auto-optimization (Performance score, a11y audit)

## Idioma

Todos los documentos, prompts y refinerencias están en **español** (según memoria de usuario).
El skill genera componentes React con comentarios en inglés (industria estándar).
