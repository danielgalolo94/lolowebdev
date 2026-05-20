# LOLOWEBDEV v1.0 → v2.0 Migration Guide
**¿Qué cambió? ¿Qué es nuevo? ¿Cómo migrarse?**

---

## 🎯 Resumen de Cambios

### ❌ Lo que Desaparece (No será usado)
- Prompt maestro único y monolítico
- Sistema de diseño flat sin domain awareness
- Componentes genéricos sin especialización
- Animaciones repetidas/predecibles

### ✅ Lo que Aparece (NUEVO)
- **UX Design Intelligence Database** → Estilos, paletas, tipografía buscables
- **Video-First Architecture** → Lenis + GSAP + Canvas rendering
- **Domain-Aware Templates** → Arquitecturas específicas por tipo de negocio
- **Design Tokens v2.0** → Configuración modular TypeScript + CSS custom properties
- **Premium Scroll Checklist** → 13 requisitos non-negotiable validados

### 🔄 Lo que Evoluciona
- **Componentes base** → Mantienen funcionalidad, reciben estilo token-driven
- **Documentación** → De 1 documento a 5 documentos especializados
- **Configuración** → De parámetros simples a interfaz uxProfile + videoSections
- **Prompt** → Del prompt maestro único a "busca en UX db + video-integration"

---

## 📊 Comparativa de Funcionalidades

| Aspecto | v1.0 | v2.0 |
|---------|------|------|
| **Estilos disponibles** | 3 (pre-configured) | 6+ (buscables en db) |
| **Paletas de color** | Flat | Dominio-aware (5+ contexts) |
| **Tipografía** | Fixed | Estratégica por rol (6 niveles) |
| **Video support** | Ninguno | Full (hero, features, testimonios, pricing) |
| **Lenis smooth scroll** | Componente | Arquitectura integrada |
| **GSAP animations** | Básico | Premium choreography (11 tipos) |
| **Canvas rendering** | No | Frame-by-frame video-to-web |
| **Domain templates** | No | 4 tipos concretos |
| **Design tokens** | CSS variables simples | TypeScript config + CSS custom |
| **Accessibility** | WCAG AA | WCAG AAA |
| **Lighthouse target** | 92 | 98+ |
| **Documentación** | 1 archivo | 5 archivos + índice |

---

## 🚀 Cómo Migrar un Proyecto Existente

### Paso 1: Actualizar Documentación Referencia
```bash
# ANTES: Consultabas lolowebdev-base.md (único)
# AHORA: Consulta lolowebdev/docs/README.md como punto de entrada

# Estructura de docs actualizado:
docs/
├── README.md (nuevo índice)
├── lolowebdev-enhanced.md (arquitectura v2.0)
├── lolowebdev-ux-guidelines.md (NUEVO)
├── lolowebdev-video-integration.md (NUEVO)
├── lolowebdev-domain-templates.md (NUEVO)
├── PHASE_1_QUICK_REFERENCE.md (NUEVO)
└── lolowebdev-base.md (histórico)
```

### Paso 2: Crear Design Tokens v2.0
```typescript
// ANTES: CSS variables simples en global.css
:root {
  --primary: #2E4036;
  --accent: #CC5833;
}

// AHORA: src/design-tokens.ts + CSS custom properties
export const DESIGN_TOKENS = {
  colors: { saas: {}, agency: {}, startup: {} },
  spacing: { xs, sm, md, lg, xl, xxl },
  typography: { fontSize, fontFamily, fontWeight, lineHeight },
  transitions: { fast, standard, slow },
  shadows: { sm, md, lg, elevation },
  radius: { sm, md, lg, xl, full },
};
```

### Paso 3: Actualizar Componentes para Token-Driven
```jsx
// ANTES:
<Button className="btn-primary" style={{ color: '#CC5833' }}>
  Click me
</Button>

// AHORA: Token-driven
<Button 
  variant="primary"
  style={{ color: 'var(--color-accent)' }}
>
  Click me
</Button>
```

### Paso 4: Preparar Estructura Video
```bash
# NUEVO: Crear estructura public/videos/
mkdir -p public/videos/{hero,features,testimonials,pricing,misc,posters,frames}

# Agregar videos reales (o placeholders para testing)
```

### Paso 5: Integrar Lenis + GSAP (si aplica)
```javascript
// NUEVO: Si usas scroll cinematográfico, ahora con Lenis
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
});
```

### Paso 6: Reconsultar Domain Template
```
// NUEVO: Identificar qué dominio es tu proyecto
// → Ver lolowebdev-domain-templates.md
// → Aplicar arquitectura de secciones específica
// → Adoptar animación choreography del dominio
```

---

## 🎨 Cambios en UX/Diseño

### v1.0: Enfoque Genérico
- Una sola paleta "Tech Premium"
- Una sola jerarquía tipográfica
- Componentes abstractos

### v2.0: Enfoque Domain-Aware
```
Input: "Soy una agencia creativa"
↓
lolowebdev busca en domain-templates.md:
  → Estética: Luxury Modern
  → Colores: Dorado + Negro + Crema
  → Tipografía: Cormorant Garamond (serif elegancia)
  → Secciones: Reel + Portfolio + Testimonios + Contact
  → Animaciones: Clip-path, horizontal reveals, rich motion
↓
Output: Sitio completamente especializado
```

---

## 🎬 Cambios en Video/Animación

### v1.0: Sin Soporte de Video
- Logos + imágenes estáticas
- Scroll cinematográfico (GSAP básico)
- Animaciones predecibles

### v2.0: Video-First Architecture
```javascript
// NUEVO: Canvas frame rendering
drawFrame(index) → image-to-scroll mapping con FRAME_SPEED 1.8-2.2

// NUEVO: Premium checklist (13 requisitos)
- Lenis smooth scroll
- Navbar pill transform
- Magnetic snap-stop
- 4+ animation types (nunca repetir)
- Staggered reveals
- Counter animations
- Marquee horizontal
- Canvas circle-wipe hero reveal
- CTA persists (data-persist="true")
- 800vh+ total scroll

// NUEVO: Video sections integradas
hero: { type: 'background', duration: '6-8s' },
features: { type: 'tab-switcher', duration: '4-5s' },
testimonios: { type: 'clips', duration: '5-8s' },
```

---

## 📈 Cambios en Performance/QA

### v1.0 Targets
- Lighthouse: 92
- Core Web Vitals: Good
- Accessibility: WCAG AA (95)

### v2.0 Targets
```
| Métrica | v1.0 | v2.0 |
|---------|------|------|
| Lighthouse | 92 | 98+ |
| Performance | 87 | 95+ |
| Accessibility | 93 | 98+ |
| Core Web Vitals | Good | Excellent |
| Contrast Ratio | 4.5:1 | 7:1 (AAA) |
| Video Load | N/A | < 2s |
```

---

## 📚 Cambios en Documentación

### v1.0: Documentación Monolítica
```
lolowebdev-base.md (1 archivo, todo mezclado)
├── ROL
├── SISTEMA DE DISEÑO
├── ARQUITECTURA DE COMPONENTES
├── PROMPTS
└── [todo junto]
```

### v2.0: Documentación Modular
```
docs/
├── README.md (índice + navegación)
├── lolowebdev-enhanced.md (arquitectura v2.0)
├── lolowebdev-ux-guidelines.md (estilos + paletas + tokens)
├── lolowebdev-video-integration.md (Lenis + GSAP + canvas)
├── lolowebdev-domain-templates.md (4 tipos de negocio)
├── PHASE_1_QUICK_REFERENCE.md (implementación ejecutable)
└── lolowebdev-base.md (histórico v1.0)
```

---

## 🔧 Cambios en Configuración de Parámetros

### v1.0: Configuración Simple
```typescript
interface LOLOWebDevConfig {
  marca: string;
  proposición: string;
  estética: 'Tech Premium' | 'Lujo Moderno' | 'Minimalismo Oscuro';
  componentes: string[];
  cta: string;
}
```

### v2.0: Configuración Domain-Aware + Video-Aware
```typescript
interface LOLOWebDevConfig {
  // Domain awareness (NUEVO)
  domain: 'saas' | 'agency' | 'startup' | 'ecommerce' | 'luxury';
  
  // Video strategy (NUEVO)
  videoSections: {
    hero?: { type: 'background' | 'clip', duration: 'short' | 'medium' | 'long' };
    features?: { type: 'tab-switcher' | 'carousel' };
    testimonios?: { type: 'clips', duration: '5-8s' };
    pricing?: { type: '3d-comparison' };
  };
  
  // UX profile (NUEVO)
  uxProfile: {
    style: 'glassmorphism' | 'minimalism' | 'brutalism' | 'luxury-modern' | 'tech-premium' | 'organic-future';
    contrastLevel: 'AA' | 'AAA';
    animationLevel: 'minimal' | 'moderate' | 'rich';
    accessibilityFirst: boolean;
  };
  
  // Existentes (mantienen compatibilidad)
  marca: string;
  proposición: string;
  cta: string;
}
```

---

## 🎯 Migración por Tipo de Proyecto

### Si es un Sitio SaaS B2B
```
ANTES (v1.0):
→ Usar lolowebdev-base.md
→ Personalizar manualmente
→ Esperar Lighthouse 92

AHORA (v2.0):
→ domain: 'saas'
→ uxProfile: { style: 'tech-premium', contrastLevel: 'AAA' }
→ Consultar lolowebdev-domain-templates.md (SaaS B2B)
→ Apuntar a Lighthouse 98+
```

### Si es un Sitio de Agencia
```
ANTES (v1.0):
→ Usar lolowebdev-base.md
→ Forzar "Lujo Moderno"
→ Menos video capability

AHORA (v2.0):
→ domain: 'agency'
→ uxProfile: { style: 'luxury-modern' }
→ videoSections: { hero: 'reel 15s', archive: 'hover-preview' }
→ Consultar lolowebdev-domain-templates.md (Agencia Creativa)
```

### Si es E-commerce Luxury
```
ANTES (v1.0):
→ Sin template especializado
→ Sin video support real
→ Animaciones genéricas

AHORA (v2.0):
→ domain: 'ecommerce'
→ videoSections: { hero: '360-product', gallery: 'hover-preview' }
→ Canvas frame rendering para producto 360°
→ Consultar lolowebdev-domain-templates.md (E-commerce Luxury)
```

---

## ✅ Checklist de Migración

### Corto Plazo (1-2 semanas)
- [ ] Leer lolowebdev-enhanced.md
- [ ] Crear design-tokens.ts v2.0
- [ ] Actualizar global.css con CSS custom properties
- [ ] Identificar dominio del proyecto
- [ ] Consultar domain-templates.md para tu tipo

### Mediano Plazo (2-4 semanas)
- [ ] Implementar Phase 1 checklist
- [ ] Audit Lighthouse/Core Web Vitals
- [ ] Refinar jerarquía visual (tokens)
- [ ] Micro-interacciones (button states, loaders)

### Largo Plazo (4-8 semanas)
- [ ] Producir/integrar videos
- [ ] Canvas frame rendering (si aplica)
- [ ] GSAP choreography premium
- [ ] Testing + optimization

---

## 🚫 Errores Comunes en Migración

### ❌ "Voy a mantener todo igual"
→ v2.0 fue diseñado para ser mejor. Aprovecha domain templates.

### ❌ "Solo cambio CSS, sin tokens"
→ Los tokens son configuración modular. Sin ellos, perderás domain-awareness.

### ❌ "Ignoro la documentación modular"
→ Los 5 documentos son interdependientes. Lee el índice en README.md.

### ❌ "Agrego video sin Lenis/GSAP"
→ La arquitectura video REQUIERE Lenis + GSAP. Sin ellos, será jerky.

### ❌ "No hago audit Lighthouse hasta el final"
→ Haz audit temprano (Phase 1). Iteraciones son más baratas.

---

## 💡 Recomendación Final

**v2.0 NO es una actualización cosmética**: es una re-arquitecturizacion completa.

**Mejor opción**: Empezar nuevo proyecto con v2.0 de cero (2-3 horas setup).
**Si tienes v1.0**: Migrarlo también vale, pero toma más tiempo debido a refactorización.

---

**Status**: 🟢 Migración v1.0 → v2.0 Documentada  
**Effort**: ~40 horas (nueva) vs ~20 horas (migración existente)  
**Benefit**: Lighthouse 98+, WCAG AAA, video-capable, domain-specialized

