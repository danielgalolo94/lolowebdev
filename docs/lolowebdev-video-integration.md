# LOLOWEBDEV Video Integration & Animation Architecture
**Referencia Integrada**: Premium Scroll-Driven Experiences con Lenis + GSAP + Canvas

---

## 📚 ÍNDICE

1. [Premium Scroll Checklist](#premium-scroll-checklist)
2. [Lenis Smooth Scroll](#lenis-setup)
3. [GSAP + ScrollTrigger Choreography](#gsap-choreography)
4. [Canvas Frame Rendering](#canvas-rendering)
5. [Animación por Tipo](#animation-types)
6. [Patrón: Navbar Pill Transform](#navbar-pill)
7. [Patrón: Magnetic Snap-Stop](#snap-stop)
8. [Patrón: Counter Animations](#counters)
9. [Patrón: Marquee Horizontal](#marquee)
10. [Checklist de Implementación](#checklist)

---

## ✅ Premium Scroll Checklist

**13 Requisitos No Negociables**:

- [ ] **Lenis Smooth Scroll** → Rueda nativa con easing personalizado (`duration: 1.2`, `easing: 1 - Math.pow(2, -10t)`)
- [ ] **Navbar Scroll-to-Pill** → Full-width → Centered pill glassmorphism en scroll
- [ ] **Magnetic Snap-Stop** → Pausa rítmica al aparición de cards (no chopping, smooth snap)
- [ ] **4+ Animation Types** → Nunca repetir consecutivamente (slide-left, slide-right, scale, clip-path, etc.)
- [ ] **Staggered Reveals** → Orden: Label (0.1s) → Heading (0.2s) → Body (0.3s) → CTA (0.4s)
- [ ] **Direction Variety** → Izq/dcha/arriba/escala/clip-path nunca en mismo bloque
- [ ] **Massive Typography** → Hero 12rem+, headings 4rem+, marquee 10vw+
- [ ] **Horizontal Marquee** → Texto 12vw+ deslizando en scroll
- [ ] **Counter Animations** → Números contando 0 → target, nunca estáticos
- [ ] **Canvas Circle-Wipe** → Hero revela via `clip-path: circle()` al scrollear
- [ ] **CTA Persists** → `data-persist="true"` mantiene visible permanentemente
- [ ] **Frame Speed 1.8-2.2** → Animación video completada al 55% scroll
- [ ] **800vh+ Total Scroll** → Hero 20%, canvas 30%, content 50%, CTA 10%

---

## 🎯 Lenis Setup

### Inicialización Base
```javascript
// 1. Verificar que Lenis está cargado
if (typeof Lenis === 'undefined') {
  console.error('Lenis library not loaded. Add CDN before app.js');
  // Fallback a scroll nativo
}

// 2. Crear instancia Lenis
const lenis = new Lenis({
  duration: 1.2,              // Suavidad base (1.0 = scroll instantáneo)
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)), // Easing exponencial
  smoothWheel: true,          // Smooth wheel scroll (no wheel events raw)
  smoothTouch: false,         // Mobile: desactivar (mejor native momentum)
  infinite: false,
});

// 3. Conectar Lenis con GSAP ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => {
  lenis.raf(time * 1000);     // RAF timestamp en ms
});
gsap.ticker.lagSmoothing(0);  // Desactivar lag smoothing
```

### Easing Functions Opcionales
```javascript
// Linear (sin suavidad)
easing: (t) => t,

// Ease-out (rápido → lento)
easing: (t) => 1 - Math.pow(1 - t, 3),

// Ease-in-out (exponencial suave)
easing: (t) => t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t,

// Custom: muy smooth
easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
```

### Mobile Considerations
```javascript
const isMobile = window.innerWidth < 768;

const lenis = new Lenis({
  duration: isMobile ? 0.8 : 1.2,  // Más rápido en mobile
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: !isMobile,           // Desactivar en mobile
  smoothTouch: false,               // Momentum nativo
});
```

---

## 🎬 GSAP + ScrollTrigger Choreography

### Timeline por Sección
```javascript
function createSectionTimeline(section) {
  const animationType = section.dataset.animation;
  const children = section.querySelectorAll('[data-animate]');
  const tl = gsap.timeline({ paused: true });
  
  // Cada tipo entra diferente
  switch (animationType) {
    case 'fade-up':
      tl.from(children, {
        y: 50,
        opacity: 0,
        stagger: 0.12,
        duration: 0.9,
        ease: 'power3.out'
      }, 0);
      break;
      
    case 'slide-left':
      tl.from(children, {
        x: -80,
        opacity: 0,
        stagger: 0.14,
        duration: 0.9,
        ease: 'power3.out'
      }, 0);
      break;
      
    case 'slide-right':
      tl.from(children, {
        x: 80,
        opacity: 0,
        stagger: 0.14,
        duration: 0.9,
        ease: 'power3.out'
      }, 0);
      break;
      
    case 'scale-up':
      tl.from(children, {
        scale: 0.85,
        opacity: 0,
        stagger: 0.12,
        duration: 1.0,
        ease: 'power2.out'
      }, 0);
      break;
      
    case 'clip-reveal':
      tl.from(children, {
        clipPath: 'inset(100% 0 0 0)',
        opacity: 0,
        stagger: 0.15,
        duration: 1.2,
        ease: 'power4.inOut'
      }, 0);
      break;
      
    case 'rotate-in':
      tl.from(children, {
        y: 40,
        rotation: 3,
        opacity: 0,
        stagger: 0.1,
        duration: 0.9,
        ease: 'power3.out'
      }, 0);
      break;
  }
  
  return tl;
}
```

### ScrollTrigger Binding
```javascript
// Vincular timeline a scroll
function bindSectionAnimation(section, timeline) {
  const enterPercent = parseFloat(section.dataset.enter) / 100;
  const leavePercent = parseFloat(section.dataset.leave) / 100;
  const persist = section.dataset.persist === 'true';
  
  ScrollTrigger.create({
    trigger: '#scroll-container',
    start: 'top top',
    end: 'bottom bottom',
    scrub: false,
    onUpdate: (self) => {
      const progress = self.progress;
      
      // Activar solo si está en rango [enter, leave]
      if (progress >= enterPercent && progress <= leavePercent) {
        const sectionProgress = (progress - enterPercent) / (leavePercent - enterPercent);
        timeline.progress(sectionProgress);
      } else if (persist && progress > leavePercent) {
        // Si persist, mantener en 100%
        timeline.progress(1);
      } else if (progress < enterPercent) {
        timeline.progress(0);
      }
    }
  });
}
```

---

## 🎥 Canvas Frame Rendering

### Extracción de Frames (Python Script)
```bash
# Extraer frames de video a WebP
python ~/.gemini/antigravity/scripts/extract_frames.py "video.mp4" 15 --remove-bg
# Output: frames/frame_0001.webp, frame_0002.webp, ...
```

### Canvas Setup
```javascript
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const frames = [];
let currentFrame = 0;
let FRAME_COUNT = 0;

// Dimensionamiento canvas
function resizeCanvas() {
  canvas.width = window.innerWidth * window.devicePixelRatio;
  canvas.height = window.innerHeight * window.devicePixelRatio;
  ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();
```

### Frame Preloader (2-Phase)
```javascript
async function preloadFrames() {
  const frameUrls = [];
  for (let i = 1; i <= FRAME_COUNT; i++) {
    frameUrls.push(`frames/frame_${String(i).padStart(4, '0')}.webp`);
  }
  
  // Fase 1: Primeras 10 frames (paint rápido)
  const firstBatch = frameUrls.slice(0, 10);
  const secondBatch = frameUrls.slice(10);
  
  // Cargar primeras 10
  const phase1Promises = firstBatch.map(url => loadImage(url));
  const phase1Frames = await Promise.all(phase1Promises);
  frames.push(...phase1Frames);
  
  // Actualizar progress bar
  updateProgressBar(10 / FRAME_COUNT);
  
  // Cargar resto en background (sin esperar)
  secondBatch.forEach(async (url, index) => {
    const img = await loadImage(url);
    frames[10 + index] = img;
    updateProgressBar((10 + index + 1) / FRAME_COUNT);
  });
}

function loadImage(src) {
  return new Promise((resolve) => {
    const img = new Image();
    img.onload = () => resolve(img);
    img.src = src;
  });
}
```

### Canvas Drawing (Padded Cover Mode)
```javascript
const IMAGE_SCALE = 0.85; // Sweet spot 0.82-0.90

function drawFrame(index) {
  const img = frames[index];
  if (!img || !img.complete) return;
  
  const cw = canvas.width / window.devicePixelRatio;
  const ch = canvas.height / window.devicePixelRatio;
  const iw = img.naturalWidth;
  const ih = img.naturalHeight;
  
  // Calcular escala (cover mode)
  const scale = Math.max(cw / iw, ch / ih) * IMAGE_SCALE;
  const dw = iw * scale;
  const dh = ih * scale;
  const dx = (cw - dw) / 2;
  const dy = (ch - dh) / 2;
  
  // Llenar canvas con color muestreado
  const bgColor = sampleBgColor(img);
  ctx.fillStyle = bgColor;
  ctx.fillRect(0, 0, cw, ch);
  
  // Dibujar imagen centrada
  ctx.drawImage(img, dx, dy, dw, dh);
}

function sampleBgColor(img) {
  // Crear canvas temporal
  const tmpCanvas = document.createElement('canvas');
  tmpCanvas.width = 1;
  tmpCanvas.height = 1;
  const tmpCtx = tmpCanvas.getContext('2d');
  
  // Muestrear esquina (asume fondo en edges)
  tmpCtx.drawImage(img, img.naturalWidth - 10, img.naturalHeight - 10, 10, 10, 0, 0, 1, 1);
  
  const imageData = tmpCtx.getImageData(0, 0, 1, 1);
  const [r, g, b] = imageData.data;
  return `rgb(${r}, ${g}, ${b})`;
}
```

### Frame-to-Scroll Binding
```javascript
const FRAME_SPEED = 2.0; // 1.8-2.2, más alto = termina antes

ScrollTrigger.create({
  trigger: '#scroll-container',
  start: 'top top',
  end: 'bottom bottom',
  scrub: true,
  onUpdate: (self) => {
    // Acelerar progreso
    const accelerated = Math.min(self.progress * FRAME_SPEED, 1);
    const index = Math.min(Math.floor(accelerated * FRAME_COUNT), FRAME_COUNT - 1);
    
    if (index !== currentFrame) {
      currentFrame = index;
      requestAnimationFrame(() => drawFrame(currentFrame));
    }
  }
});
```

---

## 🎨 Tipos de Animación

### Animation Registry
```javascript
const ANIMATION_TYPES = {
  // Entrada horizontal
  'slide-left': (child) => ({ x: -80, opacity: 0 }),
  'slide-right': (child) => ({ x: 80, opacity: 0 }),
  
  // Entrada vertical
  'fade-up': (child) => ({ y: 50, opacity: 0 }),
  'fade-down': (child) => ({ y: -50, opacity: 0 }),
  
  // Escala
  'scale-up': (child) => ({ scale: 0.85, opacity: 0 }),
  'scale-down': (child) => ({ scale: 1.15, opacity: 0 }),
  
  // Clipping
  'clip-vertical': (child) => ({ clipPath: 'inset(100% 0 0 0)', opacity: 0 }),
  'clip-horizontal': (child) => ({ clipPath: 'inset(0 100% 0 0)', opacity: 0 }),
  
  // Rotación
  'rotate-in': (child) => ({ rotation: 3, y: 40, opacity: 0 }),
  'rotate-out': (child) => ({ rotation: -3, y: -40, opacity: 0 }),
  
  // Combinados
  'bloom': (child) => ({ scale: 0.7, opacity: 0, filter: 'blur(10px)' }),
  'twist': (child) => ({ x: 100, rotation: 10, opacity: 0 }),
};
```

### Validador: Nunca Repetir Consecutivo
```javascript
function validateAnimationVariety(sections) {
  let lastAnimation = null;
  const issues = [];
  
  sections.forEach((section, index) => {
    const currentAnimation = section.dataset.animation;
    if (currentAnimation === lastAnimation) {
      issues.push(`Section ${index}: Animation "${currentAnimation}" repeated. Use different type.`);
    }
    lastAnimation = currentAnimation;
  });
  
  return issues;
}

// Uso
const sections = document.querySelectorAll('[data-animation]');
const animationIssues = validateAnimationVariety(sections);
if (animationIssues.length > 0) {
  console.warn('Animation variety issues:', animationIssues);
}
```

---

## 📱 Patrón: Navbar Pill Transform

### HTML Structure
```html
<header class="site-header" data-scroll-pill="true">
  <nav class="navbar">
    <div class="navbar-logo">Brand</div>
    <ul class="navbar-links">
      <li><a href="#features">Features</a></li>
      <li><a href="#pricing">Pricing</a></li>
    </ul>
  </nav>
</header>
```

### CSS Base
```css
.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  padding: 2rem;
}

.navbar {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  transition: all var(--transition-standard);
}

.navbar.active-pill {
  max-width: 800px;
  margin: 0 auto;
  border-radius: 50px;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 0.75rem 2rem;
}
```

### GSAP Animation
```javascript
gsap.to('.navbar', {
  maxWidth: '800px',
  borderRadius: '50px',
  marginTop: '20px',
  padding: '0.75rem 2rem',
  backgroundColor: 'rgba(255, 255, 255, 0.05)',
  backdropFilter: 'blur(15px)',
  border: '1px solid rgba(255, 255, 255, 0.1)',
  scrollTrigger: {
    trigger: 'body',
    start: '100px top',
    end: '300px top',
    scrub: true,
    onUpdate: (self) => {
      document.querySelector('.navbar').classList.toggle('active-pill', self.progress > 0.5);
    }
  }
});
```

---

## 🎯 Patrón: Magnetic Snap-Stop

### Concepto
"Snap-stop" pausa suavemente el scroll cuando una card importante aparece, creando ritmo (boom, boom, boom).

### Implementación
```javascript
const SNAP_SECTIONS = [
  { percentage: 22, duration: 0.5 },  // Snap en 22% scroll
  { percentage: 38, duration: 0.5 },  // Snap en 38% scroll
  { percentage: 54, duration: 0.5 },  // Snap en 54% scroll
];

ScrollTrigger.create({
  trigger: '#scroll-container',
  start: 'top top',
  end: 'bottom bottom',
  snap: {
    snapTo: SNAP_SECTIONS.map(s => s.percentage / 100),
    duration: 0.6,
    delay: 0.1,
    ease: 'power2.inOut'
  }
});
```

### Alternativa: Momentum Snap
```javascript
// Para mobile (no interfiera con gesture scroll)
if (!isMobile) {
  ScrollTrigger.create({
    trigger: '#scroll-container',
    snap: {
      snapTo: [0, 0.25, 0.5, 0.75, 1],
      inertia: false,
      duration: 0.8
    }
  });
}
```

---

## 🔢 Patrón: Counter Animations

### HTML
```html
<section class="section-stats">
  <div class="stat">
    <span class="stat-number" data-value="24" data-decimals="0">0</span>
    <span class="stat-suffix">hrs</span>
    <span class="stat-label">Cold retention</span>
  </div>
  <div class="stat">
    <span class="stat-number" data-value="98.5" data-decimals="1">0</span>
    <span class="stat-suffix">%</span>
    <span class="stat-label">Uptime</span>
  </div>
</section>
```

### JavaScript
```javascript
function animateCounters(section) {
  const counters = section.querySelectorAll('[data-value]');
  
  counters.forEach((counter) => {
    const targetValue = parseFloat(counter.dataset.value);
    const decimals = parseInt(counter.dataset.decimals) || 0;
    
    gsap.to(counter, {
      textContent: targetValue,
      duration: 2.0,
      ease: 'power2.out',
      snap: { textContent: 1 },
      onUpdate: function() {
        counter.textContent = (this.targets()[0]._gsap.vars.textContent || 0).toFixed(decimals);
      }
    });
  });
}

// Disparar al entrar en viewport
document.querySelectorAll('.section-stats').forEach(section => {
  ScrollTrigger.create({
    trigger: section,
    onEnter: () => animateCounters(section),
    once: true
  });
});
```

---

## 🎪 Patrón: Marquee Horizontal

### HTML
```html
<section class="marquee-section">
  <div class="marquee-wrap" data-scroll-speed="-25">
    <div class="marquee-text">Premium Experience • Premium Experience • Premium Experience</div>
  </div>
</section>
```

### CSS
```css
.marquee-wrap {
  overflow: hidden;
  white-space: nowrap;
}

.marquee-text {
  display: inline-block;
  font-size: 10vw;
  font-weight: bold;
  letter-spacing: -0.02em;
  padding-right: 2rem;
  white-space: nowrap;
}
```

### GSAP Animation
```javascript
document.querySelectorAll('.marquee-wrap').forEach(marqueeWrap => {
  const marqueeText = marqueeWrap.querySelector('.marquee-text');
  const speed = parseFloat(marqueeWrap.dataset.scrollSpeed) || -25;
  
  gsap.to(marqueeText, {
    xPercent: speed,
    ease: 'none',
    scrollTrigger: {
      trigger: '#scroll-container',
      start: 'top top',
      end: 'bottom bottom',
      scrub: true
    }
  });
  
  // Duplicar para loop seamless
  marqueeText.innerHTML += ' • ' + marqueeText.innerHTML;
});
```

---

## ✅ Checklist de Implementación

### Fase 1: Setup Base
- [ ] Lenis CDN incluido (antes de app.js)
- [ ] GSAP + ScrollTrigger CDN
- [ ] Canvas elemento en DOM
- [ ] Frames extraídos a /frames
- [ ] Loader progress bar implementado

### Fase 2: Animaciones Core
- [ ] Navbar pill transform
- [ ] Snap-stop snap mechanics
- [ ] 4+ animation types en secciones
- [ ] Staggered reveals (label → heading → body → CTA)
- [ ] Direction variety validada (sin repetir consecutivo)

### Fase 3: Canvas + Video
- [ ] Frame preloader (2-phase)
- [ ] Canvas drawing con padded cover
- [ ] Frame-to-scroll binding con FRAME_SPEED
- [ ] Circle-wipe hero reveal

### Fase 4: Content Animations
- [ ] Counter animations funcionando
- [ ] Marquee horizontal scroll
- [ ] CTA persist visible siempre
- [ ] Reducible-motion detection

### Fase 5: QA
- [ ] Mobile testing (snap desactivado)
- [ ] Lighthouse 98+
- [ ] Core Web Vitals Excellent
- [ ] Accessibility audit (WCAG AAA)
- [ ] Cross-browser (Chrome, Safari, Firefox)

---

**Status**: ✅ Integrada  
**Last Updated**: Mayo 2026  
**Source**: antigravity-video-websites-skill + lolowebdev-enhanced.md
