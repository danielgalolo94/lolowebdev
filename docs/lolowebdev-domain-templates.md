# LOLOWEBDEV Domain-Aware Templates
**Especificaciones por Tipo de Negocio**: Arquitectura, Estética, Animaciones, Tipografía

---

## 📚 ÍNDICE

1. [SaaS B2B (Tech Premium)](#saas-b2b)
2. [Agencia Creativa (Lujo Moderno)](#agencia-creativa)
3. [Startup Pre-Seed (Minimalismo Oscuro)](#startup-pre-seed)
4. [E-commerce Luxury (Monocromía)](#ecommerce-luxury)
5. [Selection Matrix](#selection-matrix)

---

## 🏢 SaaS B2B (Tech Premium)

**Target**: Empresas medianas, equipos de ingeniería, decision-makers C-level

### Estética
- **Style**: Tech Premium + Glassmorphism
- **Primary Colors**: Verde musgo (#2E4036) + Arcilla (#CC5833) + Crema (#F5F3F0)
- **Contrast Level**: AAA (WCAG maximum)
- **Animation Level**: Moderado (no overwhelm, datos primero)

### Estructura de Secciones
```
1. Hero (100vh) — Product demo video o motion graphics
   └─ Heading: "The #1 Solution for [Problem]"
   └─ Subheading: Value prop clara
   └─ CTA: "Start Free Trial" + "See Demo"
   
2. Problem/Solution (20vh) — Side-by-side texto + diagram
   └─ Icon animation: fade-up
   └─ Text: slide-left
   
3. Features Tabs (25vh) — Tab switcher con video demo
   └─ Tabs: Feature 1, Feature 2, Feature 3
   └─ Video: Play on tab select
   └─ Animation: scale-up video, fade-in text
   
4. Testimonios (20vh) — Cliente quotes con avatar video
   └─ Layout: 3 cards horizontal
   └─ Animation: stagger-up
   └─ Video circle mask: 60px radius, play on hover
   
5. Pricing (25vh) — 3 planes comparables
   └─ Plan cards con glassmorphism
   └─ Animation: slide-right, stagger 0.2s
   └─ CTA: "Get Started" highlighted
   
6. Stats (15vh) — Números impacto
   └─ Dark overlay: 0.88 opacity
   └─ Counter animation: 0 → target value (2.5s)
   └─ Format: "99%+ uptime", "50k+ users", "24/7 support"
   
7. Final CTA (10vh) — data-persist="true"
   └─ Headline + 2 CTAs
   └─ Animation: fade-up on scroll to 90%
   └─ Stays visible: nunca desaparece
```

### Typography Scale
```
h1 (Hero):     48px mobile → 64px desktop
h2 (Section):  32px mobile → 40px desktop
h3 (Feature):  24px mobile → 28px desktop
Body:          16px mobile → 18px desktop
Label:         12px mobile → 14px desktop
Data (mono):   14px mobile → 16px desktop (IBM Plex Mono)
```

### Font Stack
- **Headings (h1/h2)**: Plus Jakarta Sans Bold
- **Subheadings**: Outfit SemiBold
- **Body Text**: Inter Regular
- **Metrics/Data**: IBM Plex Mono Regular

### Animation Choreography
```
Section 1: Fade Up (label → heading → subheading)
Section 2: Slide Left (icon), Slide Right (text)
Section 3: Scale Up (video), Clip Reveal (text)
Section 4: Stagger Up (cards, 0.2s delay)
Section 5: Slide Left (plans, alternating)
Section 6: Counter animate (números)
Section 7: Fade Up (final CTA)
```

### Video Strategy
```javascript
const videoSections = {
  hero: { type: 'background', duration: '6-8s' },
  features: { type: 'tab-switcher', duration: '4-5s per tab' },
  testimonios: { type: 'clips', duration: '5-8s' },
};
```

### Sample HTML Architecture
```html
<!-- 1. Hero -->
<section class="hero-standalone" id="hero" data-enter="0" data-leave="20">
  <video autoplay muted loop poster="hero-poster.jpg">
    <source src="hero-demo.mp4" type="video/mp4">
  </video>
  <div class="hero-content">
    <h1>The #1 Solution for Ops Teams</h1>
    <p>Reduce MTTR by 70%. Deploy with confidence.</p>
    <div class="cta-group">
      <button class="cta-primary">Start Free Trial</button>
      <button class="cta-secondary">See 5-Min Demo</button>
    </div>
  </div>
</section>

<!-- 2. Features Tab -->
<section class="section-features" data-enter="35" data-leave="55" data-animation="scale-up">
  <div class="section-label">02 / Capabilities</div>
  <h2>Everything your team needs</h2>
  
  <div class="tabs">
    <button class="tab active" data-tab="feature-1">Real-time Monitoring</button>
    <button class="tab" data-tab="feature-2">Smart Alerts</button>
    <button class="tab" data-tab="feature-3">Integrations</button>
  </div>
  
  <div class="tab-content active" id="feature-1">
    <video src="feature-1-demo.mp4"></video>
    <p>Monitor 10k+ metrics in real-time with <1ms latency.</p>
  </div>
</section>

<!-- 3. Testimonios -->
<section class="section-testimonios" data-enter="58" data-leave="72" data-animation="stagger-up">
  <div class="section-label">03 / Customers</div>
  <h2>Loved by teams at</h2>
  
  <div class="testimonios-grid">
    <div class="testimonial-card">
      <video src="customer-1.mp4" class="avatar-video"></video>
      <p>"Cut our incident response time by 60%."</p>
      <div class="author">Jane CEO, TechCorp</div>
    </div>
  </div>
</section>

<!-- 4. Pricing -->
<section class="section-pricing" data-enter="75" data-leave="95" data-animation="slide-right">
  <div class="section-label">04 / Plans</div>
  <h2>Simple, transparent pricing</h2>
  
  <div class="pricing-cards">
    <div class="card starter">
      <h3>Starter</h3>
      <p class="price">$29<span>/mo</span></p>
      <ul>
        <li>Up to 5 servers</li>
        <li>Core monitoring</li>
      </ul>
      <button>Get Started</button>
    </div>
    <div class="card pro featured">
      <h3>Pro</h3>
      <p class="price">$99<span>/mo</span></p>
      <ul>
        <li>Unlimited servers</li>
        <li>Smart alerts</li>
        <li>Integrations</li>
      </ul>
      <button>Get Started</button>
    </div>
  </div>
</section>

<!-- 5. Stats -->
<section class="section-stats" data-enter="97" data-leave="100" data-animation="stagger-up">
  <div class="stats-overlay"></div>
  <div class="stat">
    <span class="stat-number" data-value="99.99" data-decimals="2">0</span>
    <span class="stat-suffix">%</span>
    <span class="stat-label">Uptime SLA</span>
  </div>
  <div class="stat">
    <span class="stat-number" data-value="50000" data-decimals="0">0</span>
    <span class="stat-suffix">+</span>
    <span class="stat-label">Active Users</span>
  </div>
</section>

<!-- 6. Final CTA -->
<section class="section-cta" data-persist="true" data-animation="fade-up">
  <h2>Ready to optimize your operations?</h2>
  <p>Join 50k+ teams shipping faster, safer.</p>
  <button class="cta-primary cta-large">Start Free Trial</button>
</section>
```

### CSS Key Rules
```css
:root {
  --color-primary: #2E4036;
  --color-accent: #CC5833;
  --color-surface: #F5F3F0;
  --color-text: #1a1a1a;
  --color-text-muted: #666;
}

/* Glassmorphism cards */
.card {
  background: rgba(255, 255, 255, 0.10);
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 12px;
}

/* Side-aligned text (no center) */
.section-features {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
}
.section-features > div:first-child {
  align-self: center;
  padding-right: 4rem;
}

/* Stats overlay */
.stats-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.88);
  z-index: -1;
}
```

---

## 🎨 Agencia Creativa (Lujo Moderno)

**Target**: Marcas de lujo, fashion, lifestyle, arquitectura

### Estética
- **Style**: Luxury Modern + Serif Typography
- **Primary Colors**: Dorado (#D4A574) + Negro (#1a1a1a) + Crema (#F5F3F0)
- **Contrast Level**: AAA
- **Animation Level**: Rich (movimiento constantemente)

### Estructura de Secciones
```
1. Hero (30vh) — Full-screen motion graphics reel
   └─ Video: 15s loop, silent, autoplay
   └─ Overlay: Tipo serif dorado elegante
   
2. Archive/Portfolio (40vh) — Grid de proyectos
   └─ Cada proyecto: hover reveal video preview
   └─ Animation: Clip-path reveal horizontal
   
3. Manifiesto (20vh) — Textual vision
   └─ Tipografía serif italic
   └─ Animation: Staggered line-by-line reveal
   
4. Testimonios Clientes (15vh) — Video persona clips
   └─ Full-width video players
   └─ Animation: Fade in on scroll
   
5. Contact (10vh) — data-persist="true"
   └─ Email form minimalista
   └─ Animation: Subtle fade
```

### Typography Scale
```
h1 (Hero):     64px mobile → 96px desktop (serif)
h2 (Section):  40px mobile → 56px desktop (serif italic)
h3 (Project):  24px mobile → 32px desktop (serif)
Body:          16px mobile → 18px desktop (sans serif)
Tagline:       14px mobile → 16px desktop (serif italic)
```

### Font Stack
- **Headings (h1/h2/h3)**: Cormorant Garamond (regular + italic)
- **Body Text**: Inter Regular
- **Taglines**: Cormorant Garamond Italic

### Animation Choreography
```
Section 1: Fade Up + Scale (video entrance)
Section 2: Clip-path horizontal reveal (project cards)
Section 3: Line-by-line clip reveal (manifiesto texto)
Section 4: Fade In (testimonial videos)
Section 5: Subtle fade up (contact form)
```

### Video Strategy
```javascript
const videoSections = {
  hero: { type: 'background', duration: '15s loop' },
  archive: { type: 'hover-preview', duration: '4-6s' },
  testimonios: { type: 'full-width', duration: '8-12s' },
};
```

### Sample HTML
```html
<section class="hero-standalone" id="hero">
  <video autoplay muted loop>
    <source src="reel-cinematic.mp4" type="video/mp4">
  </video>
  <div class="hero-text">
    <h1>Creative Excellence</h1>
    <p class="tagline"><em>Crafted for brands that dare</em></p>
  </div>
</section>

<section class="archive" data-enter="25" data-leave="65" data-animation="clip-reveal">
  <div class="projects-grid">
    <div class="project-card">
      <div class="project-image">
        <img src="project-1.jpg">
        <video src="project-1-preview.mp4" class="preview-video"></video>
      </div>
      <h3>Luxury Watch Campaign</h3>
      <p>Directorial approach to chronometry</p>
    </div>
  </div>
</section>

<section class="manifiesto" data-enter="68" data-leave="80" data-animation="clip-reveal">
  <h2 style="font-style: italic;">Our philosophy</h2>
  <p>We believe in craft that transcends trends.</p>
  <p>Every frame deliberate. Every moment earned.</p>
</section>
```

---

## 🚀 Startup Pre-Seed (Minimalismo Oscuro)

**Target**: Y Combinator alumni, VC-backed, tech-forward founders

### Estética
- **Style**: Minimalism + Tech Premium
- **Primary Colors**: Neon (#00FF88) + Charcoal (#1F1F1F) + Blanco (#FFFFFF)
- **Contrast Level**: AAA (máximo)
- **Animation Level**: Minimal (datos, no distracción)

### Estructura de Secciones
```
1. Hero (15vh) — Motion graphics abstracto
   └─ Heading: 3.5rem mono "The problem statement"
   └─ Animation: Fade up staggered words
   
2. Problem (12vh) — Textual, side-aligned
   └─ Icon + description
   └─ Animation: Slide left
   
3. Solution (12vh) — Diagram + video parallax
   └─ Layout: Image left, text right
   └─ Animation: Scale up diagram
   
4. Beta Signup (15vh) — Form + testimonios early adopters (video)
   └─ Form: Email only
   └─ Social proof: 3 video avatares
   └─ Animation: Stagger up
   
5. Roadmap (20vh) — Timeline con milestones
   └─ Cada milestone: icon + video snippet
   └─ Animation: Counter + clip reveal
   
6. Final CTA (10vh) — data-persist="true"
   └─ "Join the waitlist"
   └─ Animation: Fade
```

### Typography Scale
```
h1 (Hero):     40px mobile → 56px desktop (monospace)
h2 (Section):  28px mobile → 40px desktop (monospace)
h3 (Feature):  20px mobile → 24px desktop (sans serif)
Body:          16px mobile → 18px desktop (sans serif)
Code/Data:     14px mobile → 16px desktop (monospace)
```

### Font Stack
- **Headings**: Outfit Bold (monospace feel via weight)
- **Body**: Inter Regular
- **Data**: IBM Plex Mono

### Animation Choreography
```
Section 1: Fade Up (word by word)
Section 2: Slide Left (icon + text)
Section 3: Scale Up (diagram/video)
Section 4: Stagger Up (form + avatars)
Section 5: Counter + Clip Reveal (roadmap)
Section 6: Fade (CTA)
```

### Video Strategy
```javascript
const videoSections = {
  hero: { type: 'abstract-motion', duration: '6-8s' },
  solution: { type: 'parallax', duration: '10-15s' },
  betaSignup: { type: 'avatar-clips', duration: '5-8s' },
  roadmap: { type: 'milestone-snippets', duration: '3-4s each' },
};
```

### Sample HTML
```html
<section class="hero-standalone" id="hero">
  <h1>
    <span>The future of</span>
    <span>autonomous</span>
    <span>infrastructure.</span>
  </h1>
  <p>We're building the OS for cloud-native teams.</p>
  <button>Join Waitlist</button>
</section>

<section class="beta-signup" data-enter="50" data-leave="65" data-animation="stagger-up">
  <div class="section-label">Early Adopters</div>
  <h2>Join 500+ builders</h2>
  
  <form>
    <input type="email" placeholder="your@email.com">
    <button>Get Early Access</button>
  </form>
  
  <div class="social-proof">
    <div class="avatar-with-video">
      <video src="founder-1.mp4"></video>
      <p>"Game-changing" — CEO, TechStartup</p>
    </div>
  </div>
</section>

<section class="roadmap" data-enter="68" data-leave="88" data-animation="clip-reveal">
  <h2>What's Next</h2>
  <div class="timeline">
    <div class="milestone">
      <span class="month">May 2026</span>
      <h4>MVP Launch</h4>
      <video src="milestone-1.mp4"></video>
    </div>
  </div>
</section>
```

---

## 💎 E-commerce Luxury (Monocromía)

**Target**: Alta joyería, moda de lujo, accesorios premium

### Estética
- **Style**: Luxury Modern + Monocromía o Gradiente
- **Primary Colors**: Monocromo Negro (gradiente #000 → #333) O Gradiente Saturado
- **Contrast Level**: AAA
- **Animation Level**: Rich (cinematografía producto)

### Estructura de Secciones
```
1. Hero (25vh) — Producto 360° video
   └─ Video: Full-screen rotatable
   └─ Overlay: Precio + "Discover"
   
2. Gallery (30vh) — Grid de productos con hover preview
   └─ Thumbnail → Tap/hover → Full video
   └─ Animation: Zoom in producto
   
3. Story (15vh) — Heritage/craftsmanship narración
   └─ Text + subtle video background
   └─ Animation: Text fade, video autoplay
   
4. Social Proof (15vh) — Customer unboxing videos
   └─ 3 clips lado a lado
   └─ Animation: Stagger fade
   
5. Testimonios (10vh) — Ratings + short video clips
   └─ Stars + quote + face video
   └─ Animation: Fade up
   
6. Shop CTA (10vh) — data-persist="true"
   └─ "Shop the collection"
   └─ Product counter: x items in stock
```

### Typography Scale
```
h1 (Hero):     48px mobile → 72px desktop
h2 (Section):  32px mobile → 48px desktop
h3 (Product):  24px mobile → 32px desktop
Body:          16px mobile → 18px desktop
Price:         20px mobile → 24px desktop (bold)
```

### Font Stack
- **Headings**: Cormorant Garamond (serif elegancia)
- **Body**: Inter Regular
- **Price**: Inter Bold

### Animation Choreography
```
Section 1: Scale Up (video entrada)
Section 2: Zoom In (producto hover)
Section 3: Fade In (texto), autoplay video
Section 4: Stagger Fade (unboxing videos)
Section 5: Fade Up (testimonios)
Section 6: Subtle fade (CTA)
```

### Video Strategy
```javascript
const videoSections = {
  hero: { type: '360-product', duration: '10-15s loop' },
  gallery: { type: 'product-hover', duration: '4-6s each' },
  story: { type: 'background-subtle', duration: '15-20s' },
  socialProof: { type: 'unboxing-clips', duration: '8-12s' },
};
```

### Sample HTML
```html
<section class="hero-standalone" id="hero">
  <video autoplay muted loop>
    <source src="product-360.mp4" type="video/mp4">
  </video>
  <div class="hero-overlay">
    <h1>The Vesper Collection</h1>
    <p class="price">$3,500</p>
    <button>Discover</button>
  </div>
</section>

<section class="gallery" data-enter="28" data-leave="58">
  <h2>Signature Pieces</h2>
  <div class="products-grid">
    <div class="product-card" data-animation="zoom-in">
      <img src="product-1.jpg">
      <video src="product-1-video.mp4" class="product-video"></video>
      <h3>Eternal Pendant</h3>
      <p class="price">$2,800</p>
    </div>
  </div>
</section>

<section class="social-proof" data-enter="62" data-leave="75" data-animation="stagger-fade">
  <h2>Unboxing Moments</h2>
  <div class="videos-grid">
    <video src="unboxing-1.mp4"></video>
    <video src="unboxing-2.mp4"></video>
    <video src="unboxing-3.mp4"></video>
  </div>
</section>

<section class="shop-cta" data-persist="true" data-animation="fade-up">
  <h2>Shop the Collection</h2>
  <p><span class="stock-counter" data-value="47">0</span> pieces available</p>
  <button class="cta-primary cta-large">View All</button>
</section>
```

---

## 🎯 Selection Matrix

| Dominio | Estilo | Paleta | Tipografía | Animaciones | Videos |
|---------|--------|--------|-----------|------------|--------|
| **SaaS B2B** | Tech Premium | Verde + Arcilla | Plus Jakarta Sans + Inter | Moderado (fade/slide) | Demo + testimonios |
| **Agencia** | Luxury Modern | Dorado + Negro | Cormorant Garamond | Rich (clip-path) | Reel + portfolio |
| **Startup** | Minimalism | Neon + Charcoal | Outfit Mono + Inter | Minimal (fade/counter) | Motion graphics + avatares |
| **E-commerce** | Luxury Modern | Monocromía | Cormorant Garamond + Inter | Rich (zoom/scale) | Producto 360° + unboxing |

---

## 🚀 Deployment Checklist por Dominio

### SaaS B2B
- [ ] Product demo video comprimido (< 10MB)
- [ ] Testimonios clip compilado (5-8s cada)
- [ ] Feature tab video links (4-5s)
- [ ] Stats counters animando
- [ ] Navbar pill funcionando en mobile

### Agencia Creativa
- [ ] Motion graphics reel 15s
- [ ] Portfolio project previews cargando lazy
- [ ] Manifiesto clip-reveal texto
- [ ] Testimonios videos streaming smooth
- [ ] Gold accent colors WCAG AAA

### Startup Pre-Seed
- [ ] Hero motion graphics renderizado
- [ ] Beta form validando
- [ ] Roadmap counters & timeline animando
- [ ] Early adopter videos cargando
- [ ] Waitlist tracker actualizado

### E-commerce Luxury
- [ ] Product 360 video loop smooth
- [ ] Unboxing videos gallery
- [ ] Stock counter animando
- [ ] Monocromía/gradiente rendering crisp
- [ ] Price display prominente

---

**Status**: ✅ Integradas  
**Last Updated**: Mayo 2026  
**Source**: lolowebdev-enhanced.md + domain-aware-generation
