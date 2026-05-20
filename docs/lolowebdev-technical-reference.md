# LOLOWEBDEV - Technical Reference Guide

## Quick Start (Para usar el Skill)

```bash
# En cualquier conversación, escribí:
/lolowebdev

# O menciona palabras clave:
"creá una landing cinematográfica para mi SaaS"
"necesito un sitio web apple-style"
"genera un instrumento digital premium"
```

---

## Anatomía de un Componente LOLOWEBDEV

### Patrón Base (Hero Component)

```jsx
'use client';

import { useEffect } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default function Hero() {
  useEffect(() => {
    // CRUCIAL: envolve en gsap.context() para limpieza
    const ctx = gsap.context(() => {
      // Fade-up escalonado
      const tl = gsap.timeline();
      
      tl.from('.hero-title', {
        opacity: 0,
        y: 40,
        duration: 0.8,
        ease: 'power2.out'
      })
      .from('.hero-subtitle', {
        opacity: 0,
        y: 20,
        duration: 0.6,
        ease: 'power2.out'
      }, '-=0.4')
      .from('.hero-cta', {
        opacity: 0,
        y: 10,
        duration: 0.5,
        ease: 'power2.out'
      }, '-=0.3');

      // Parallax sutil en imagen de fondo
      gsap.to('.hero-bg', {
        y: -100,
        scrollTrigger: {
          trigger: '.hero',
          start: 'top top',
          end: 'bottom top',
          scrub: 1, // smooth scrub
          markers: false
        }
      });
    });

    return () => ctx.revert(); // IMPORTANTE: cleanup
  }, []);

  return (
    <section className="hero relative h-screen overflow-hidden">
      {/* Fondo con imagen */}
      <div 
        className="hero-bg absolute inset-0 bg-cover bg-center"
        style={{
          backgroundImage: 'url(https://images.unsplash.com/...)',
          backgroundSize: 'cover'
        }}
      >
        {/* Overlay degradado */}
        <div className="absolute inset-0 bg-gradient-to-br from-[#2E4036]/60 via-transparent to-black/40" />
      </div>

      {/* Contenido posicionado en tercio inferior */}
      <div className="relative h-full flex flex-col justify-end p-8 md:p-16">
        <div className="w-full md:w-2/3 lg:w-1/2">
          <h1 className="hero-title">
            <span className="block font-['Plus Jakarta Sans'] font-bold text-4xl md:text-6xl text-white leading-tight">
              Tu problema es el
            </span>
            <span className="block font-['Cormorant Garamond'] italic text-6xl md:text-7xl text-white mt-4">
              Algoritmo
            </span>
          </h1>

          <p className="hero-subtitle mt-6 text-lg md:text-xl text-gray-100 font-['Inter'] max-w-md">
            Automatiza decisiones con IA que entiende tu contexto.
          </p>

          <button className="hero-cta mt-8 px-8 py-4 bg-[#CC5833] text-white rounded-full font-['Plus Jakarta Sans'] font-bold hover:scale-105 transition-transform">
            Reserva demo
          </button>
        </div>
      </div>
    </section>
  );
}
```

---

## Hooks Reutilizables

### 1. useScrollTrigger (Aparición en Scroll)

```jsx
// src/hooks/useScrollTrigger.js
import { useEffect } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export const useScrollTrigger = (ref, animation = {}) => {
  const {
    from = { opacity: 0, y: 50 },
    duration = 0.6,
    trigger = 'top 80%',
    ease = 'power2.out'
  } = animation;

  useEffect(() => {
    if (!ref.current) return;

    const ctx = gsap.context(() => {
      gsap.from(ref.current, {
        ...from,
        duration,
        scrollTrigger: {
          trigger: ref.current,
          start: `top ${trigger}`,
          markers: false
        },
        ease
      });
    });

    return () => ctx.revert();
  }, []);

  return ref;
};

// USO:
// const ref = useRef(null);
// useScrollTrigger(ref, { from: { opacity: 0, x: -50 }, duration: 0.8 });
// <div ref={ref}>Contenido</div>
```

### 2. useParallax (Movimiento de Fondo)

```jsx
// src/hooks/useParallax.js
import { useEffect } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export const useParallax = (ref, factor = 0.3) => {
  useEffect(() => {
    if (!ref.current) return;

    const ctx = gsap.context(() => {
      gsap.to(ref.current, {
        y: -window.innerHeight * factor,
        scrollTrigger: {
          trigger: ref.current,
          start: 'top center',
          end: 'bottom center',
          scrub: 1,
          markers: false
        }
      });
    });

    return () => ctx.revert();
  }, [factor]);

  return ref;
};

// USO: const bgRef = useParallax(useRef(null), 0.5);
```

### 3. useStackedCards (Baraja Automática)

```jsx
// src/hooks/useStackedCards.js
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

export const useStackedCards = (cardsRef, interval = 3000) => {
  useEffect(() => {
    if (!cardsRef.current) return;

    const cards = cardsRef.current.children;
    if (cards.length === 0) return;

    const ctx = gsap.context(() => {
      const rotate = () => {
        const tl = gsap.timeline();

        // Card 0 se sale arriba
        tl.to(cards[0], {
          y: -300,
          opacity: 0,
          duration: 0.8,
          ease: 'power2.inOut'
        }, 0);

        // Cards[1] y [2] suben
        for (let i = 1; i < cards.length; i++) {
          const targetScale = i === 1 ? 1 : 0.95;
          const targetBlur = i === 1 ? 0 : 8;
          const targetOpacity = i === 1 ? 1 : 0.6;

          tl.to(cards[i], {
            scale: targetScale,
            filter: `blur(${targetBlur}px)`,
            opacity: targetOpacity,
            duration: 0.6,
            ease: 'power2.out'
          }, 0);
        }

        // Nueva card entra abajo
        const newCard = cards[0].cloneNode(true);
        cardsRef.current.appendChild(newCard);

        setTimeout(() => {
          Array.from(cards).forEach(card => card.remove());
          rotate();
        }, interval);
      };

      rotate();
    });

    return () => ctx.revert();
  }, [interval]);
};
```

---

## Componentes Avanzados

### Feature: Stacked Cards Carousel

```jsx
'use client';

import { useEffect, useRef } from 'react';
import gsap from 'gsap';

export function StackedCards({ items = [] }) {
  const containerRef = useRef(null);

  useEffect(() => {
    if (!containerRef.current) return;
    
    const ctx = gsap.context(() => {
      const cards = containerRef.current.querySelectorAll('[data-card]');
      let currentIndex = 0;

      const rotate = () => {
        const tl = gsap.timeline();

        cards.forEach((card, idx) => {
          const isActive = idx === currentIndex % cards.length;
          const isNext = idx === (currentIndex + 1) % cards.length;

          if (isActive) {
            tl.to(card, {
              y: 0,
              scale: 1,
              opacity: 1,
              filter: 'blur(0px)',
              duration: 0.6,
              ease: 'back.out(1.7)'
            }, 0);
          } else if (isNext) {
            tl.to(card, {
              y: 20,
              scale: 0.95,
              opacity: 0.6,
              filter: 'blur(8px)',
              duration: 0.6,
              ease: 'power2.out'
            }, 0);
          } else {
            tl.to(card, {
              y: 40,
              scale: 0.9,
              opacity: 0.3,
              filter: 'blur(15px)',
              duration: 0.6,
              ease: 'power2.out'
            }, 0);
          }
        });

        currentIndex++;
        setTimeout(rotate, 3500);
      };

      rotate();
    });

    return () => ctx.revert();
  }, []);

  return (
    <div className="relative w-full h-96 flex items-center justify-center">
      <div ref={containerRef} className="relative w-full max-w-sm h-72">
        {items.map((item, idx) => (
          <div
            key={idx}
            data-card
            className="absolute inset-0 bg-white rounded-2xl p-6 shadow-lg flex flex-col justify-between"
            style={{
              y: idx === 0 ? 0 : idx === 1 ? 20 : 40,
              scale: idx === 0 ? 1 : idx === 1 ? 0.95 : 0.9,
              opacity: idx === 0 ? 1 : idx === 1 ? 0.6 : 0.3,
              filter: idx === 0 ? 'blur(0px)' : idx === 1 ? 'blur(8px)' : 'blur(15px)'
            }}
          >
            <div className="text-3xl font-bold text-[#2E4036]">{item.metric}</div>
            <div className="text-sm text-gray-600 font-['Plus Jakarta Sans']">{item.label}</div>
          </div>
        ))}
      </div>
    </div>
  );
}
```

### Feature: Terminal Typing Effect

```jsx
'use client';

import { useEffect, useState } from 'react';

const MESSAGES = [
  'Optimizando sistema...',
  'Analizando tendencias...',
  'Generando variantes...',
  'Procesando datos...',
  'Preparando resultados...'
];

export function Terminal() {
  const [displayText, setDisplayText] = useState('');
  const [messageIdx, setMessageIdx] = useState(0);
  const [charIdx, setCharIdx] = useState(0);
  const [isDeleting, setIsDeleting] = useState(false);

  useEffect(() => {
    const currentMessage = MESSAGES[messageIdx];
    const timer = setTimeout(() => {
      if (!isDeleting) {
        if (charIdx < currentMessage.length) {
          setDisplayText(currentMessage.slice(0, charIdx + 1));
          setCharIdx(charIdx + 1);
        } else {
          // Pausa antes de borrar
          setTimeout(() => setIsDeleting(true), 2000);
        }
      } else {
        if (charIdx > 0) {
          setDisplayText(currentMessage.slice(0, charIdx - 1));
          setCharIdx(charIdx - 1);
        } else {
          setIsDeleting(false);
          setMessageIdx((prev) => (prev + 1) % MESSAGES.length);
        }
      }
    }, isDeleting ? 50 : 100);

    return () => clearTimeout(timer);
  }, [charIdx, messageIdx, isDeleting]);

  return (
    <div className="bg-[#1A1A1A] rounded-2xl p-6 font-['IBM Plex Mono'] text-sm text-[#2E4036] border border-[#CC5833]/20">
      <div className="flex items-center gap-2">
        <span>▌</span>
        <span>{displayText}</span>
      </div>
      <div className="mt-4 flex items-center gap-2">
        <div className="w-2 h-2 bg-[#CC5833] rounded-full animate-pulse" />
        <span className="text-xs text-gray-400">EN VIVO</span>
      </div>
    </div>
  );
}
```

### Feature: Calendar Ghost Cursor

```jsx
'use client';

import { useEffect, useState } from 'react';
import gsap from 'gsap';

export function AutoCalendar() {
  const [selectedDay, setSelectedDay] = useState(null);
  const cursorRef = useRef(null);

  useEffect(() => {
    const ctx = gsap.context(() => {
      const days = Array.from({ length: 31 }, (_, i) => i + 1);
      let currentDay = null;

      const automateClick = () => {
        // Elige día aleatorio
        currentDay = Math.floor(Math.random() * 31) + 1;
        const dayElement = document.querySelector(`[data-day="${currentDay}"]`);

        if (!dayElement) return;

        // Anima cursor hacia el día
        gsap.to(cursorRef.current, {
          x: dayElement.offsetLeft + dayElement.offsetWidth / 2,
          y: dayElement.offsetTop + dayElement.offsetHeight / 2,
          duration: 1,
          ease: 'power2.inOut',
          onComplete: () => {
            // Simula click
            gsap.to(dayElement, {
              scale: 0.8,
              duration: 0.2
            });
            setSelectedDay(currentDay);

            // Después de pausa, vuelve a empezar
            setTimeout(automateClick, 5000);
          }
        });
      };

      automateClick();
    });

    return () => ctx.revert();
  }, []);

  return (
    <div className="relative w-full max-w-sm mx-auto">
      <div 
        ref={cursorRef}
        className="absolute w-6 h-6 bg-[#CC5833] rounded-full pointer-events-none z-50"
        style={{ transform: 'translate(-50%, -50%)' }}
      />

      <div className="bg-white rounded-2xl p-6">
        <div className="grid grid-cols-7 gap-2">
          {Array.from({ length: 31 }, (_, i) => i + 1).map(day => (
            <button
              key={day}
              data-day={day}
              className={`p-2 rounded-lg text-sm font-bold transition-all ${
                selectedDay === day
                  ? 'bg-[#2E4036] text-white'
                  : 'bg-gray-100 text-[#1A1A1A] hover:bg-gray-200'
              }`}
            >
              {day}
            </button>
          ))}
        </div>

        <button className="w-full mt-6 py-3 bg-[#CC5833] text-white rounded-full font-bold">
          Guardar
        </button>
      </div>
    </div>
  );
}
```

---

## Configuración Global (globals.css)

```css
/* Design Tokens */
:root {
  /* Colores Tech Premium */
  --color-primary: #2E4036;
  --color-accent: #CC5833;
  --color-bg: #F2F0E9;
  --color-dark: #1A1A1A;
  --color-light: #FFFFFF;

  /* Tipografía */
  --font-sans: 'Plus Jakarta Sans', system-ui, sans-serif;
  --font-serif: 'Cormorant Garamond', serif;
  --font-mono: 'IBM Plex Mono', monospace;

  /* Spacing */
  --spacing-unit: 1rem;
}

/* Smooth Scroll Global */
html {
  scroll-behavior: smooth;
}

/* Noise Overlay Sutil */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image: 
    url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100"><filter id="noise"><feTurbulence type="fractalNoise" baseFrequency="0.5" numOctaves="4" seed="2"/></filter><rect width="100" height="100" filter="url(%23noise)" opacity="0.05"/></svg>');
  pointer-events: none;
  opacity: 0.05;
  z-index: 9999;
}

/* Prevenir reduce-motion */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* Scroll suave en ScrollTrigger */
.smooth-scroll {
  scroll-behavior: smooth;
}
```

---

## Estructura Next.js (package.json)

```json
{
  "name": "lolowebdev-site",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "next": "^15.0.0",
    "gsap": "^3.12.2",
    "framer-motion": "^11.0.0",
    "lucide-react": "latest",
    "typed.js": "^2.1.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "tailwindcss": "^4.0.0",
    "postcss": "latest",
    "autoprefixer": "latest",
    "@types/react": "^19.0.0",
    "@types/node": "latest"
  }
}
```

---

## Optimization Tips

### 1. Lazy Load Imágenes

```jsx
<Image
  src="/hero-bg.webp"
  alt="Hero Background"
  priority
  fill
  sizes="100vw"
  className="object-cover"
/>
```

### 2. Code Splitting Automático (Next.js)

```jsx
// Solo cargar componentes que se ven en viewport
import dynamic from 'next/dynamic';

const Archive = dynamic(() => import('@/components/Archive'), {
  loading: () => <div className="h-screen bg-gray-100" />
});
```

### 3. Core Web Vitals

```jsx
// next.config.js
export default {
  images: {
    formats: ['image/avif', 'image/webp'],
    remotePatterns: [
      { hostname: 'images.unsplash.com' }
    ]
  },
  // Compress SVGs
  webpack: (config) => {
    config.module.rules.push({
      test: /\.svg$/,
      use: ['@svgr/webpack']
    });
    return config;
  }
};
```

---

## Deploy (Vercel)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Alias (producción)
vercel --prod
```

---

**Última actualización**: 2026-05-20
**Versión**: 1.0 Técnica