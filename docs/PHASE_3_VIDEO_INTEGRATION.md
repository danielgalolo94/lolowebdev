# LOLOWEBDEV v2.0 - Phase 3: Video Integration
**Canvas Rendering + Lenis/GSAP Choreography + Performance Optimization**

---

## 🎯 Phase 3 Objetivos

✅ Implementar canvas frame rendering (video-to-scroll)  
✅ Integrar Lenis + GSAP premium choreography  
✅ Crear componentes video React con lazy-loading  
✅ Optimizar rendimiento (compression, CDN, caching)  
✅ Testing cross-browser y Core Web Vitals

**Timeline**: Semana 5-6  
**Status**: 🟡 Ready to Start  
**Dependencies**: Phase 1 ✅ + Phase 2 ✅

---

## 📋 Tarea 1: Canvas Frame Rendering Setup

### 1.1 Extracción de Frames (FFmpeg)

```bash
#!/bin/bash
# scripts/extract-frames.sh
# Extraer frames de video a WebP

VIDEO_PATH=$1
OUTPUT_DIR=${2:-./public/frames}
TARGET_FPS=${3:-15}

if [ -z "$VIDEO_PATH" ]; then
  echo "Usage: ./extract-frames.sh <video_path> [output_dir] [fps]"
  exit 1
fi

# Crear directorio
mkdir -p "$OUTPUT_DIR"

# Extraer frames con FFmpeg
# H.264 MP4 → WebP (mejor compresión que PNG)
ffmpeg -i "$VIDEO_PATH" \
  -vf "fps=$TARGET_FPS" \
  -c:v libwebp \
  -quality 80 \
  -loop 0 \
  "$OUTPUT_DIR/frame_%04d.webp"

echo "✅ Frames extracted to $OUTPUT_DIR"
echo "Target: ${TARGET_FPS}fps"

# Contar frames
FRAME_COUNT=$(ls "$OUTPUT_DIR"/frame_*.webp 2>/dev/null | wc -l)
echo "Total frames: $FRAME_COUNT"
```

```bash
# Uso
chmod +x scripts/extract-frames.sh
./scripts/extract-frames.sh public/videos/hero-demo.mp4 public/frames/hero 15
# Output: public/frames/hero/frame_0001.webp, frame_0002.webp, ...
```

### 1.2 Canvas Setup & Configuration

```typescript
// src/components/CanvasRenderer.tsx

interface CanvasConfig {
  frameDir: string;
  frameCount: number;
  frameSpeed: number; // 1.8-2.2
  scale: number;     // 0.82-0.90 (IMAGE_SCALE)
}

interface CanvasRendererProps {
  config: CanvasConfig;
  trigger: string; // Scroll trigger selector
}

export function CanvasRenderer({ config, trigger }: CanvasRendererProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const [frames, setFrames] = useState<HTMLImageElement[]>([]);
  const [currentFrame, setCurrentFrame] = useState(0);
  const [progress, setProgress] = useState(0);

  // 1. Canvas setup
  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    // Responsive sizing
    const resizeCanvas = () => {
      canvas.width = window.innerWidth * window.devicePixelRatio;
      canvas.height = window.innerHeight * window.devicePixelRatio;
      ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
    };

    resizeCanvas();
    window.addEventListener('resize', resizeCanvas);
    return () => window.removeEventListener('resize', resizeCanvas);
  }, []);

  // 2. Frame preloader (2-phase)
  useEffect(() => {
    const preloadFrames = async () => {
      const loadedFrames: HTMLImageElement[] = [];

      // Fase 1: Load first 10 frames immediately
      const firstBatch = Array.from(
        { length: Math.min(10, config.frameCount) },
        (_, i) => `${config.frameDir}/frame_${String(i + 1).padStart(4, '0')}.webp`
      );

      const phase1Promises = firstBatch.map((url) => loadImage(url));
      const phase1Frames = await Promise.all(phase1Promises);
      loadedFrames.push(...phase1Frames);

      // Update progress
      setProgress((10 / config.frameCount) * 100);

      // Fase 2: Load remaining in background
      const remainingBatch = Array.from(
        { length: config.frameCount - 10 },
        (_, i) => `${config.frameDir}/frame_${String(i + 11).padStart(4, '0')}.webp`
      );

      remainingBatch.forEach((url, index) => {
        loadImage(url).then((img) => {
          loadedFrames[10 + index] = img;
          setProgress(((10 + index + 1) / config.frameCount) * 100);
        });
      });

      setFrames(loadedFrames);
    };

    preloadFrames();
  }, [config]);

  return (
    <div className="canvas-container">
      <canvas
        ref={canvasRef}
        className="canvas-renderer"
        role="img"
        aria-label="Animated video canvas"
      />
      {progress < 100 && (
        <div className="loading-bar">
          <div className="loading-progress" style={{ width: `${progress}%` }} />
        </div>
      )}
    </div>
  );
}

function loadImage(src: string): Promise<HTMLImageElement> {
  return new Promise((resolve) => {
    const img = new Image();
    img.onload = () => resolve(img);
    img.onerror = () => {
      console.error(`Failed to load: ${src}`);
      resolve(new Image()); // Return empty image on error
    };
    img.src = src;
  });
}
```

### 1.3 Canvas Drawing (Padded Cover Mode)

```typescript
// src/lib/canvas-draw.ts

const IMAGE_SCALE = 0.85; // Sweet spot 0.82-0.90

export function drawFrame(
  ctx: CanvasRenderingContext2D,
  img: HTMLImageElement,
  canvas: HTMLCanvasElement
) {
  if (!img || !img.complete) return;

  const cw = canvas.width / window.devicePixelRatio;
  const ch = canvas.height / window.devicePixelRatio;
  const iw = img.naturalWidth;
  const ih = img.naturalHeight;

  // Cover mode: escala para cubrir canvas
  const scale = Math.max(cw / iw, ch / ih) * IMAGE_SCALE;
  const dw = iw * scale;
  const dh = ih * scale;
  const dx = (cw - dw) / 2;
  const dy = (ch - dh) / 2;

  // Sample background color from frame corners
  const bgColor = sampleBgColor(img);

  // Fill canvas with sampled color (fills padding)
  ctx.fillStyle = bgColor;
  ctx.fillRect(0, 0, cw, ch);

  // Draw image centered
  ctx.drawImage(img, dx, dy, dw, dh);
}

function sampleBgColor(img: HTMLImageElement): string {
  const tmpCanvas = document.createElement('canvas');
  tmpCanvas.width = 1;
  tmpCanvas.height = 1;
  const tmpCtx = tmpCanvas.getContext('2d');
  if (!tmpCtx) return '#ffffff';

  // Sample from corner (assumes solid bg at edges)
  tmpCtx.drawImage(img, img.naturalWidth - 10, img.naturalHeight - 10, 10, 10, 0, 0, 1, 1);

  const imageData = tmpCtx.getImageData(0, 0, 1, 1);
  const [r, g, b] = imageData.data;
  return `rgb(${r}, ${g}, ${b})`;
}

// Re-sample every 20 frames for dynamic color changes
export function shouldResampleColor(frameIndex: number): boolean {
  return frameIndex % 20 === 0;
}
```

### 1.4 Frame-to-Scroll Binding (GSAP ScrollTrigger)

```typescript
// src/lib/canvas-scroll-binding.ts

import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

interface FrameScrollBindingConfig {
  canvas: HTMLCanvasElement;
  frames: HTMLImageElement[];
  trigger: string; // '#scroll-container'
  frameSpeed: number; // 1.8-2.2
  onFrame?: (index: number) => void;
}

export function bindFrameToScroll(config: FrameScrollBindingConfig) {
  const { canvas, frames, trigger, frameSpeed, onFrame } = config;
  const ctx = canvas.getContext('2d');
  if (!ctx) return;

  let currentFrame = 0;
  const FRAME_COUNT = frames.length;

  ScrollTrigger.create({
    trigger,
    start: 'top top',
    end: 'bottom bottom',
    scrub: true, // Smooth scrub to scroll
    onUpdate: (self) => {
      // Accelerate progress
      const accelerated = Math.min(self.progress * frameSpeed, 1);
      const index = Math.min(
        Math.floor(accelerated * FRAME_COUNT),
        FRAME_COUNT - 1
      );

      if (index !== currentFrame) {
        currentFrame = index;
        
        requestAnimationFrame(() => {
          drawFrame(ctx, frames[currentFrame], canvas);
          onFrame?.(currentFrame);
        });
      }
    },
  });
}

// Resample color periodically
export function startColorResampler(
  canvas: HTMLCanvasElement,
  frames: HTMLImageElement[],
  interval: number = 20
) {
  let frameIndex = 0;
  
  const resampler = setInterval(() => {
    frameIndex++;
    if (frameIndex > frames.length) {
      clearInterval(resampler);
    }
  }, 1000 / 24); // ~24fps check
}
```

---

## 📋 Tarea 2: Lenis + GSAP Integration

### 2.1 Lenis Setup (Next.js)

```typescript
// src/lib/lenis.ts

import { useEffect } from 'react';
import Lenis from '@lenis/react';
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export function useLenis() {
  useEffect(() => {
    // Verificar Lenis loaded
    if (typeof Lenis === 'undefined') {
      console.warn('Lenis not loaded. Add CDN before app renders.');
      return;
    }

    const lenis = new Lenis({
      duration: 1.2,
      easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
      smoothWheel: true,
      smoothTouch: false,
      infinite: false,
    });

    // Connect to ScrollTrigger
    lenis.on('scroll', ScrollTrigger.update);

    // RAF loop
    gsap.ticker.add((time) => {
      lenis.raf(time * 1000);
    });

    gsap.ticker.lagSmoothing(0);

    return () => {
      lenis.destroy();
    };
  }, []);
}
```

```tsx
// src/app/layout.tsx

import { useLenis } from '@/lib/lenis';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  useLenis();

  return (
    <html>
      <body>{children}</body>
    </html>
  );
}
```

### 2.2 GSAP Choreography Patterns

```typescript
// src/lib/gsap-choreography.ts

import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

// Animation type registry
export const ANIMATION_TYPES = {
  'fade-up': (children: Element[]) => ({
    y: 50,
    opacity: 0,
    stagger: 0.12,
    duration: 0.9,
    ease: 'power3.out',
  }),
  'slide-left': (children: Element[]) => ({
    x: -80,
    opacity: 0,
    stagger: 0.14,
    duration: 0.9,
    ease: 'power3.out',
  }),
  'slide-right': (children: Element[]) => ({
    x: 80,
    opacity: 0,
    stagger: 0.14,
    duration: 0.9,
    ease: 'power3.out',
  }),
  'scale-up': (children: Element[]) => ({
    scale: 0.85,
    opacity: 0,
    stagger: 0.12,
    duration: 1.0,
    ease: 'power2.out',
  }),
  'clip-reveal': (children: Element[]) => ({
    clipPath: 'inset(100% 0 0 0)',
    opacity: 0,
    stagger: 0.15,
    duration: 1.2,
    ease: 'power4.inOut',
  }),
  'rotate-in': (children: Element[]) => ({
    y: 40,
    rotation: 3,
    opacity: 0,
    stagger: 0.1,
    duration: 0.9,
    ease: 'power3.out',
  }),
};

interface SectionAnimationConfig {
  section: Element;
  animationType: keyof typeof ANIMATION_TYPES;
}

export function setupSectionAnimation(config: SectionAnimationConfig) {
  const { section, animationType } = config;
  const children = section.querySelectorAll('[data-animate]');
  
  const timeline = gsap.timeline({ paused: true });
  const animationProps = ANIMATION_TYPES[animationType](Array.from(children));

  timeline.from(children, animationProps, 0);

  // Bind to ScrollTrigger
  const enterPercent = parseFloat(section.getAttribute('data-enter') || '0') / 100;
  const leavePercent = parseFloat(section.getAttribute('data-leave') || '100') / 100;
  const persist = section.getAttribute('data-persist') === 'true';

  ScrollTrigger.create({
    trigger: '#scroll-container',
    start: 'top top',
    end: 'bottom bottom',
    scrub: false,
    onUpdate: (self) => {
      const progress = self.progress;

      if (progress >= enterPercent && progress <= leavePercent) {
        const sectionProgress = (progress - enterPercent) / (leavePercent - enterPercent);
        timeline.progress(sectionProgress);
      } else if (persist && progress > leavePercent) {
        timeline.progress(1);
      } else if (progress < enterPercent) {
        timeline.progress(0);
      }
    },
  });

  return timeline;
}

// Validar variedad de animaciones (nunca repetir consecutivo)
export function validateAnimationVariety(sections: Element[]): string[] {
  const issues: string[] = [];
  let lastAnimation: string | null = null;

  sections.forEach((section, index) => {
    const currentAnimation = section.getAttribute('data-animation');
    if (currentAnimation === lastAnimation) {
      issues.push(
        `Section ${index}: Animation "${currentAnimation}" repeated. Use different type.`
      );
    }
    lastAnimation = currentAnimation;
  });

  return issues;
}
```

### 2.3 Navbar Pill Transform

```tsx
// src/components/NavbarPill.tsx

import { useEffect } from 'react';
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';
import styles from './NavbarPill.module.css';

gsap.registerPlugin(ScrollTrigger);

export function NavbarPill() {
  useEffect(() => {
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
          const navbar = document.querySelector('.navbar');
          if (navbar) {
            navbar.classList.toggle('active-pill', self.progress > 0.5);
          }
        },
      },
    });
  }, []);

  return (
    <header className={styles.header}>
      <nav className={`${styles.navbar} navbar`}>
        <div className={styles.logo}>Brand</div>
        <ul className={styles.links}>
          <li><a href="#features">Features</a></li>
          <li><a href="#pricing">Pricing</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>
      </nav>
    </header>
  );
}
```

```css
/* src/components/NavbarPill.module.css */

.header {
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
}

.logo {
  font-weight: bold;
  font-size: 1.25rem;
}

.links {
  display: flex;
  list-style: none;
  gap: 2rem;
}

.links a {
  text-decoration: none;
  transition: color var(--transition-fast);
}

.links a:hover {
  color: var(--color-accent);
}
```

---

## 📋 Tarea 3: Video React Components

### 3.1 Hero Video Component

```tsx
// src/components/HeroVideo.tsx

import { useRef, useEffect, useState } from 'react';
import styles from './HeroVideo.module.css';

interface HeroVideoProps {
  src: string;
  webmSrc?: string;
  poster?: string;
  title: string;
  subtitle?: string;
}

export function HeroVideo({ src, webmSrc, poster, title, subtitle }: HeroVideoProps) {
  const videoRef = useRef<HTMLVideoElement>(null);
  const [isPlaying, setIsPlaying] = useState(true);

  useEffect(() => {
    const video = videoRef.current;
    if (!video) return;

    // Autoplay muted
    video.muted = true;
    video.play().catch(() => {
      setIsPlaying(false);
      // Fallback to poster if autoplay fails
    });
  }, []);

  return (
    <section className={styles.hero}>
      <video
        ref={videoRef}
        className={styles.video}
        autoPlay
        muted
        loop
        playsInline
        poster={poster}
        aria-label={title}
      >
        {webmSrc && <source src={webmSrc} type="video/webm" />}
        <source src={src} type="video/mp4" />
        Your browser does not support HTML5 video.
      </video>

      <div className={styles.overlay} />

      <div className={styles.content}>
        <h1 className={styles.title} data-animate>{title}</h1>
        {subtitle && (
          <p className={styles.subtitle} data-animate>{subtitle}</p>
        )}
        <button
          className={styles.playButton}
          onClick={() => {
            if (videoRef.current) {
              if (videoRef.current.paused) {
                videoRef.current.play();
                setIsPlaying(true);
              } else {
                videoRef.current.pause();
                setIsPlaying(false);
              }
            }
          }}
          aria-label={isPlaying ? 'Pause video' : 'Play video'}
        >
          {isPlaying ? '⏸' : '▶'}
        </button>
      </div>
    </section>
  );
}
```

```css
/* src/components/HeroVideo.module.css */

.hero {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.3),
    rgba(0, 0, 0, 0.1)
  );
  z-index: 1;
}

.content {
  position: relative;
  z-index: 2;
  text-align: center;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.title {
  font-size: clamp(2.5rem, 10vw, 5rem);
  font-weight: 700;
  line-height: 1.1;
  max-width: 90%;
}

.subtitle {
  font-size: 1.25rem;
  opacity: 0.9;
}

.playButton {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  border: 2px solid white;
  color: white;
  font-size: 2rem;
  cursor: pointer;
  transition: all var(--transition-standard);
  backdrop-filter: blur(10px);
}

.playButton:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

.playButton:active {
  transform: scale(0.95);
}
```

### 3.2 Feature Tab Video Component

```tsx
// src/components/FeatureTabVideo.tsx

import { useState } from 'react';
import styles from './FeatureTabVideo.module.css';

interface Feature {
  id: string;
  label: string;
  title: string;
  description: string;
  videoSrc: string;
  poster: string;
}

interface FeatureTabVideoProps {
  features: Feature[];
}

export function FeatureTabVideo({ features }: FeatureTabVideoProps) {
  const [activeTab, setActiveTab] = useState(features[0]?.id || '');
  const activeFeature = features.find((f) => f.id === activeTab);

  if (!activeFeature) return null;

  return (
    <section className={styles.container}>
      <div className={styles.tabs}>
        {features.map((feature) => (
          <button
            key={feature.id}
            className={`${styles.tab} ${activeTab === feature.id ? styles.active : ''}`}
            onClick={() => setActiveTab(feature.id)}
            aria-selected={activeTab === feature.id}
            aria-controls={`panel-${feature.id}`}
          >
            {feature.label}
          </button>
        ))}
      </div>

      <div className={styles.content}>
        <div className={styles.videoWrapper}>
          <video
            key={activeFeature.id}
            className={styles.video}
            poster={activeFeature.poster}
            controls
            playsInline
            aria-label={`${activeFeature.title} demo`}
          >
            <source src={activeFeature.videoSrc} type="video/mp4" />
          </video>
        </div>

        <div className={styles.text}>
          <h3 className={styles.title} data-animate>
            {activeFeature.title}
          </h3>
          <p className={styles.description} data-animate>
            {activeFeature.description}
          </p>
        </div>
      </div>
    </section>
  );
}
```

---

## 📋 Tarea 4: Performance Optimization

### 4.1 Video Compression Strategy

```bash
#!/bin/bash
# scripts/compress-videos.sh

VIDEO_INPUT=$1
QUALITY=${2:-23}  # 18 (best) to 28 (worst), default 23

if [ -z "$VIDEO_INPUT" ]; then
  echo "Usage: ./compress-videos.sh <input.mp4> [quality]"
  exit 1
fi

FILENAME=$(basename "$VIDEO_INPUT" .mp4)

echo "Compressing $FILENAME..."

# MP4 H.264 (broader compatibility)
ffmpeg -i "$VIDEO_INPUT" \
  -c:v libx264 \
  -preset medium \
  -crf $QUALITY \
  -c:a aac \
  -b:a 128k \
  "output/${FILENAME}_h264.mp4"

# WebM VP9 (better compression)
ffmpeg -i "$VIDEO_INPUT" \
  -c:v libvpx-vp9 \
  -crf 30 \
  -b:v 0 \
  -c:a libopus \
  "output/${FILENAME}_vp9.webm"

# Extract poster frame
ffmpeg -i "$VIDEO_INPUT" \
  -ss 00:00:01 \
  -vframes 1 \
  -q:v 5 \
  "output/${FILENAME}_poster.jpg"

echo "✅ Compression complete"
echo "Files in output/"
ls -lh output/"
```

### 4.2 Lazy Loading with Intersection Observer

```tsx
// src/components/LazyVideo.tsx

import { useRef, useEffect, useState } from 'react';

interface LazyVideoProps {
  src: string;
  poster: string;
  alt: string;
}

export function LazyVideo({ src, poster, alt }: LazyVideoProps) {
  const videoRef = useRef<HTMLVideoElement>(null);
  const [isVisible, setIsVisible] = useState(false);

  useEffect(() => {
    const video = videoRef.current;
    if (!video) return;

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsVisible(true);
          // Autoplay when visible
          video.play().catch(() => {
            console.log('Autoplay prevented');
          });
        } else {
          setIsVisible(false);
          video.pause();
        }
      },
      { threshold: 0.5 }
    );

    observer.observe(video);
    return () => observer.disconnect();
  }, []);

  return (
    <video
      ref={videoRef}
      poster={poster}
      controls
      playsInline
      preload={isVisible ? 'auto' : 'none'}
      aria-label={alt}
    >
      <source src={src} type="video/mp4" />
    </video>
  );
}
```

### 4.3 CDN & Caching Headers

```typescript
// next.config.js

/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    domains: ['cdn.example.com'],
    formats: ['image/avif', 'image/webp'],
  },
  
  // Video CDN headers
  headers: async () => [
    {
      source: '/videos/:path*',
      headers: [
        {
          key: 'Cache-Control',
          value: 'public, max-age=31536000, immutable', // 1 year
        },
        {
          key: 'Content-Type',
          value: 'video/mp4',
        },
      ],
    },
    {
      source: '/frames/:path*',
      headers: [
        {
          key: 'Cache-Control',
          value: 'public, max-age=31536000, immutable',
        },
      ],
    },
  ],
};

module.exports = nextConfig;
```

---

## 📋 Tarea 5: Testing & QA

### 5.1 Performance Metrics

```bash
# Lighthouse audit
npm run lighthouse https://localhost:3000

# Expected Phase 3 scores:
# ✅ Performance: 95+
# ✅ Accessibility: 98+
# ✅ Core Web Vitals: Excellent
#   - CLS: < 0.05
#   - FID: < 100ms
#   - LCP: < 2.5s
#   - TTFB: < 0.6s
```

### 5.2 Cross-browser Testing

```
Browsers:
✅ Chrome 120+ (reference)
✅ Safari 17+ (iOS 17+)
✅ Firefox 121+
✅ Edge 120+

Devices:
✅ iPhone 14/15
✅ Samsung Galaxy S23
✅ iPad Pro 12.9"
✅ MacBook Pro 14"
✅ Desktop 1920x1080
✅ Desktop 2560x1440 (ultrawide)
```

### 5.3 Video Playback Checklist

- [ ] MP4 + WebM fallback tested
- [ ] Poster frame displays before load
- [ ] Autoplay works on desktop
- [ ] Fallback to poster on mobile
- [ ] Controls functional (play, pause, volume, fullscreen)
- [ ] Frame canvas renders correctly
- [ ] Lenis scroll smooth (no jank)
- [ ] GSAP animations sync with scroll
- [ ] No memory leaks (DevTools)
- [ ] Battery drain acceptable (iOS Simulator)

---

## ✅ Phase 3 Checklist

### Canvas Rendering
- [ ] Frame extraction script (FFmpeg)
- [ ] Canvas setup with responsive sizing
- [ ] Frame preloader (2-phase)
- [ ] Canvas drawing (padded cover mode)
- [ ] Frame-to-scroll binding with FRAME_SPEED
- [ ] Color resampling (dynamic)

### Lenis + GSAP
- [ ] Lenis initialized (1.2s duration)
- [ ] ScrollTrigger registered
- [ ] Navbar pill transform working
- [ ] 4+ animation types implemented
- [ ] Stagger reveals working
- [ ] Animation variety validated (no repeats)

### Video Components
- [ ] HeroVideo component (autoplay, muted, loop)
- [ ] FeatureTabVideo component (tab switching)
- [ ] LazyVideo component (intersection observer)
- [ ] Play buttons functional
- [ ] Accessibility labels complete

### Performance
- [ ] Video compression (MP4 + WebM)
- [ ] Lazy loading active (defer off-viewport)
- [ ] Cache headers configured (1 year CDN)
- [ ] Frame preloader optimized
- [ ] Lighthouse 95+ achieved
- [ ] Core Web Vitals Excellent

### Testing
- [ ] Cross-browser verified (Chrome, Safari, Firefox, Edge)
- [ ] Mobile viewport tested (iPhone, Android)
- [ ] Tablet tested (iPad, Galaxy Tab)
- [ ] Desktop ultrawide tested (2560x1440)
- [ ] No console errors
- [ ] No memory leaks
- [ ] Battery drain acceptable

---

## 🚀 Siguientes Pasos (Phase 4)

Una vez completado Phase 3:

**Fase 4: Testing + Polish (Semana 7-8)**
- [ ] A/B testing (con/sin video)
- [ ] Performance profiling
- [ ] Production deployment
- [ ] Monitoring & analytics setup

---

**Status**: 🟡 Ready to Implement  
**Estimated Duration**: 2-3 semanas  
**Dependencies**: Phase 1 ✅ + Phase 2 ✅  
**Owner**: Video Integration Team

