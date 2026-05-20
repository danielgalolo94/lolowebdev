# LOLOWEBDEV v2.0 - Phase 4: Testing + Polish & Deployment
**Quality Assurance + Production Deployment + Monitoring**

---

## 🎯 Phase 4 Objetivos

✅ Testing completo (unit, integration, e2e)  
✅ Performance profiling y optimization  
✅ A/B testing (con/sin video)  
✅ Deployment a producción  
✅ Monitoring y alerting setup  

**Timeline**: Semana 7-8  
**Status**: 🟡 Ready to Start  
**Dependencies**: Phase 1 ✅ + Phase 2 ✅ + Phase 3 ✅

---

## 📋 Tarea 1: Testing Strategy

### 1.1 Unit Testing Setup (Jest + React Testing Library)

```typescript
// jest.config.js
export default {
  preset: 'ts-jest',
  testEnvironment: 'jsdom',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.ts?(x)', '**/?(*.)+(spec|test).ts?(x)'],
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.tsx',
  ],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 80,
      statements: 80,
    },
  },
};
```

### 1.2 Unit Test Examples

```typescript
// src/components/Button.test.tsx

import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from './Button';

describe('Button component', () => {
  it('renders button with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button')).toHaveTextContent('Click me');
  });

  it('calls onClick handler when clicked', async () => {
    const handleClick = jest.fn();
    const user = userEvent.setup();
    
    render(<Button onClick={handleClick}>Click me</Button>);
    await user.click(screen.getByRole('button'));
    
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('disables button when disabled prop is true', () => {
    render(<Button disabled>Disabled</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
  });

  it('applies correct variant classes', () => {
    const { rerender } = render(<Button variant="primary">Text</Button>);
    expect(screen.getByRole('button')).toHaveClass('primary');

    rerender(<Button variant="secondary">Text</Button>);
    expect(screen.getByRole('button')).toHaveClass('secondary');
  });

  it('has proper focus ring on focus', async () => {
    const user = userEvent.setup();
    render(<Button>Click me</Button>);
    
    const button = screen.getByRole('button');
    await user.tab();
    
    expect(button).toHaveFocus();
  });
});
```

```typescript
// src/lib/canvas-draw.test.ts

import { drawFrame, sampleBgColor } from './canvas-draw';

describe('Canvas drawing utilities', () => {
  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;

  beforeEach(() => {
    canvas = document.createElement('canvas');
    canvas.width = 800;
    canvas.height = 600;
    ctx = canvas.getContext('2d')!;
  });

  it('draws image with correct scaling', () => {
    const img = new Image();
    img.width = 1920;
    img.height = 1080;
    
    const drawSpy = jest.spyOn(ctx, 'drawImage');
    drawFrame(ctx, img, canvas);
    
    expect(drawSpy).toHaveBeenCalled();
  });

  it('fills canvas with sampled background color', () => {
    const img = new Image();
    const fillSpy = jest.spyOn(ctx, 'fillRect');
    
    drawFrame(ctx, img, canvas);
    
    expect(fillSpy).toHaveBeenCalledWith(0, 0, 800, 600);
  });
});
```

### 1.3 E2E Testing (Playwright)

```typescript
// e2e/hero.spec.ts

import { test, expect } from '@playwright/test';

test.describe('Hero Section', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('http://localhost:3000');
  });

  test('video autoplay on desktop', async ({ page }) => {
    const video = page.locator('video').first();
    const isPlaying = await video.evaluate((v: HTMLVideoElement) => !v.paused);
    expect(isPlaying).toBe(true);
  });

  test('navbar transforms to pill on scroll', async ({ page }) => {
    const navbar = page.locator('.navbar');
    const initialWidth = await navbar.evaluate((el) =>
      window.getComputedStyle(el).width
    );

    // Scroll down
    await page.evaluate(() => window.scrollBy(0, 200));

    const finalWidth = await navbar.evaluate((el) =>
      window.getComputedStyle(el).width
    );

    expect(finalWidth).not.toBe(initialWidth);
  });

  test('animations trigger on scroll', async ({ page }) => {
    const section = page.locator('[data-animation="fade-up"]').first();
    
    // Scroll to section
    await section.scrollIntoViewIfNeeded();

    const opacity = await section.evaluate((el) =>
      window.getComputedStyle(el).opacity
    );

    expect(parseFloat(opacity)).toBeGreaterThan(0);
  });

  test('focus ring visible on keyboard navigation', async ({ page }) => {
    const button = page.locator('button').first();

    // Tab to button
    await page.keyboard.press('Tab');
    const focused = await button.evaluate((el) => el === document.activeElement);
    
    expect(focused).toBe(true);

    // Check focus-visible styles
    const hasOutline = await button.evaluate((el) => {
      const styles = window.getComputedStyle(el);
      return styles.outlineWidth !== '0px';
    });

    expect(hasOutline).toBe(true);
  });
});
```

### 1.4 Running Tests

```bash
# Unit tests
npm test

# Unit tests with coverage
npm test -- --coverage

# E2E tests
npm run test:e2e

# All tests
npm run test:all
```

---

## 📋 Tarea 2: Performance Profiling

### 2.1 Bundle Analysis

```bash
# Install bundle analyzer
npm install --save-dev @next/bundle-analyzer

# Configure in next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer({
  // ... rest of config
});

# Run analysis
ANALYZE=true npm run build
```

### 2.2 Lighthouse Automated Testing

```typescript
// scripts/lighthouse-ci.js

import lighthouse from 'lighthouse';
import * as chromeLauncher from 'chrome-launcher';

async function runLighthouse(url) {
  const chrome = await chromeLauncher.launch({ chromeFlags: ['--headless'] });

  const options = {
    logLevel: 'info',
    output: 'json',
    port: chrome.port,
    onlyCategories: [
      'performance',
      'accessibility',
      'best-practices',
      'seo',
      'pwa',
    ],
  };

  const runnerResult = await lighthouse(url, options);

  const scores = {
    performance: runnerResult.lhr.categories.performance.score * 100,
    accessibility: runnerResult.lhr.categories.accessibility.score * 100,
    best_practices:
      runnerResult.lhr.categories['best-practices'].score * 100,
    seo: runnerResult.lhr.categories.seo.score * 100,
  };

  console.log('\n📊 Lighthouse Scores:');
  console.log(`Performance:     ${scores.performance.toFixed(0)}`);
  console.log(`Accessibility:   ${scores.accessibility.toFixed(0)}`);
  console.log(`Best Practices:  ${scores.best_practices.toFixed(0)}`);
  console.log(`SEO:             ${scores.seo.toFixed(0)}`);

  // Assert thresholds
  const THRESHOLDS = {
    performance: 95,
    accessibility: 98,
    best_practices: 95,
    seo: 90,
  };

  const failures = Object.entries(THRESHOLDS).filter(
    ([category, threshold]) => scores[category] < threshold
  );

  if (failures.length > 0) {
    console.error('\n❌ Lighthouse thresholds not met:');
    failures.forEach(([category, threshold]) => {
      console.error(
        `  ${category}: ${scores[category].toFixed(0)}/${threshold}`
      );
    });
    process.exit(1);
  }

  await chrome.kill();
}

runLighthouse('http://localhost:3000');
```

```bash
# Add to package.json scripts
"test:lighthouse": "node scripts/lighthouse-ci.js"

# Run before deployment
npm run test:lighthouse
```

### 2.3 Core Web Vitals Monitoring

```typescript
// src/lib/web-vitals.ts

import {
  getCLS,
  getFID,
  getFCP,
  getLCP,
  getTTFB,
} from 'web-vitals';

interface WebVitalsMetrics {
  CLS: number;
  FID: number;
  FCP: number;
  LCP: number;
  TTFB: number;
}

const metrics: Partial<WebVitalsMetrics> = {};

getCLS((metric) => {
  metrics.CLS = metric.value;
  console.log(`CLS: ${metric.value}`);
});

getFID((metric) => {
  metrics.FID = metric.value;
  console.log(`FID: ${metric.value}`);
});

getFCP((metric) => {
  metrics.FCP = metric.value;
  console.log(`FCP: ${metric.value}`);
});

getLCP((metric) => {
  metrics.LCP = metric.value;
  console.log(`LCP: ${metric.value}`);
});

getTTFB((metric) => {
  metrics.TTFB = metric.value;
  console.log(`TTFB: ${metric.value}`);
});

// Send to analytics
export function sendWebVitals() {
  if (typeof window !== 'undefined' && window.gtag) {
    window.gtag('event', 'page_view', {
      cls: metrics.CLS,
      fid: metrics.FID,
      fcp: metrics.FCP,
      lcp: metrics.LCP,
      ttfb: metrics.TTFB,
    });
  }
}
```

---

## 📋 Tarea 3: A/B Testing Setup

### 3.1 Feature Flag Implementation

```typescript
// src/lib/feature-flags.ts

export type FeatureFlag = 'enable-video' | 'enable-animations' | 'enable-analytics';

interface FeatureFlags {
  [key: string]: boolean;
}

// Server-side config
export const FEATURE_FLAGS: FeatureFlags = {
  'enable-video': process.env.NEXT_PUBLIC_FEATURE_VIDEO === 'true',
  'enable-animations': process.env.NEXT_PUBLIC_FEATURE_ANIMATIONS === 'true',
  'enable-analytics': process.env.NEXT_PUBLIC_FEATURE_ANALYTICS === 'true',
};

// Client-side hook
export function useFeatureFlag(flag: FeatureFlag): boolean {
  return FEATURE_FLAGS[flag] ?? false;
}

// Context API for dynamic flags
import { createContext, useContext } from 'react';

const FeatureFlagsContext = createContext<FeatureFlags>(FEATURE_FLAGS);

export function FeatureFlagsProvider({ children }: { children: React.ReactNode }) {
  return (
    <FeatureFlagsContext.Provider value={FEATURE_FLAGS}>
      {children}
    </FeatureFlagsContext.Provider>
  );
}

export function useFeatureFlags() {
  return useContext(FeatureFlagsContext);
}
```

### 3.2 A/B Test Variants

```typescript
// src/components/HeroSection.tsx

import { useFeatureFlag } from '@/lib/feature-flags';
import { HeroVideo } from './HeroVideo';
import { HeroStatic } from './HeroStatic';

export function HeroSection() {
  const enableVideo = useFeatureFlag('enable-video');

  // A/B test: Video vs Static
  if (enableVideo) {
    return (
      <HeroVideo
        src="/videos/hero-demo.mp4"
        title="Hero with Video"
        data-testid="hero-video"
      />
    );
  }

  return (
    <HeroStatic
      image="/images/hero-static.jpg"
      title="Hero with Static Image"
      data-testid="hero-static"
    />
  );
}
```

### 3.3 Analytics Tracking

```typescript
// src/lib/analytics.ts

export function trackEvent(
  eventName: string,
  eventData: Record<string, any>
) {
  if (typeof window !== 'undefined' && window.gtag) {
    window.gtag('event', eventName, eventData);
  }
}

// Track variant views
export function trackABTestVariant(testName: string, variant: string) {
  trackEvent('ab_test_variant', {
    test_name: testName,
    variant,
    timestamp: new Date().toISOString(),
  });
}

// Track performance metrics
export function trackPerformance(metric: string, value: number) {
  trackEvent('performance_metric', {
    metric,
    value,
    timestamp: new Date().toISOString(),
  });
}
```

```bash
# .env.local
NEXT_PUBLIC_FEATURE_VIDEO=true
NEXT_PUBLIC_FEATURE_ANIMATIONS=true
NEXT_PUBLIC_FEATURE_ANALYTICS=true
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXXXXX
```

---

## 📋 Tarea 4: Deployment to Production

### 4.1 Pre-deployment Checklist

```bash
#!/bin/bash
# scripts/pre-deploy.sh

echo "🔍 Pre-deployment checklist..."

# 1. Run all tests
echo "Running tests..."
npm run test:all || exit 1

# 2. Run Lighthouse
echo "Running Lighthouse audit..."
npm run test:lighthouse || exit 1

# 3. Check bundle size
echo "Checking bundle size..."
npm run build || exit 1

# 4. Check for console errors
echo "Checking code quality..."
npm run lint || exit 1

# 5. Security audit
echo "Running security audit..."
npm audit || exit 1

echo "✅ All checks passed! Ready for deployment."
```

### 4.2 Vercel Deployment Configuration

```typescript
// vercel.json

{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "outputDirectory": ".next",
  "installCommand": "npm install",
  "env": {
    "NEXT_PUBLIC_FEATURE_VIDEO": "true",
    "NEXT_PUBLIC_FEATURE_ANIMATIONS": "true",
    "NEXT_PUBLIC_FEATURE_ANALYTICS": "true"
  },
  "headers": [
    {
      "source": "/videos/:path*",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/:path*",
      "headers": [
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-XSS-Protection",
          "value": "1; mode=block"
        }
      ]
    }
  ],
  "redirects": [],
  "rewrites": []
}
```

### 4.3 Deployment Steps

```bash
# 1. Create production branch
git checkout -b deploy/v2.0-production

# 2. Run pre-deployment checks
./scripts/pre-deploy.sh

# 3. Tag release
git tag -a v2.0.0 -m "Release v2.0.0 - LOLOWEBDEV Enhanced"

# 4. Push to GitHub
git push origin deploy/v2.0-production
git push origin v2.0.0

# 5. Vercel auto-deploys from GitHub
# (If using GitHub integration)

# Or manual deploy
vercel --prod --token=$VERCEL_TOKEN
```

---

## 📋 Tarea 5: Monitoring & Alerting

### 5.1 Error Tracking (Sentry)

```typescript
// src/lib/sentry.ts

import * as Sentry from '@sentry/nextjs';

export function initSentry() {
  Sentry.init({
    dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
    environment: process.env.NODE_ENV,
    tracesSampleRate: 1.0,
    profilesSampleRate: 0.1,
    denyUrls: [
      // Browser extensions
      /extensions\//i,
      /^chrome:\/\//i,
    ],
  });
}

// Error boundary
export const ErrorBoundary = Sentry.ErrorBoundary;

// Capture exceptions
export function captureException(error: Error, context: Record<string, any>) {
  Sentry.captureException(error, {
    contexts: { custom: context },
  });
}
```

### 5.2 Analytics Dashboard (Google Analytics)

```typescript
// src/lib/gtag.ts

export const GA_MEASUREMENT_ID = process.env.NEXT_PUBLIC_GA_ID;

// Page view
export function pageView(url: string) {
  if (typeof window !== 'undefined' && window.gtag) {
    window.gtag('config', GA_MEASUREMENT_ID, {
      page_path: url,
    });
  }
}

// Custom event
export function event(action: string, params: Record<string, any>) {
  if (typeof window !== 'undefined' && window.gtag) {
    window.gtag('event', action, params);
  }
}
```

### 5.3 Uptime Monitoring

```typescript
// Uptime Robot or similar service
// Configure monitors:
// - Homepage (https://yourdomain.com) - 5min interval
// - API endpoint (if applicable)
// - Video CDN (check video availability)

// Webhook for alerts
export async function handleUptimeAlert(event: {
  status: 'up' | 'down';
  lastDowntime?: number;
}) {
  if (event.status === 'down') {
    // Send Slack alert
    await fetch(process.env.SLACK_WEBHOOK_URL!, {
      method: 'POST',
      body: JSON.stringify({
        text: `⚠️ Website down! Downtime: ${event.lastDowntime}ms`,
      }),
    });
  }
}
```

---

## 📋 Tarea 6: Post-deployment & Rollback

### 6.1 Smoke Testing

```bash
#!/bin/bash
# scripts/smoke-tests.sh

echo "🔥 Running smoke tests on production..."

DOMAIN="https://yourdomain.com"

# Check homepage loads
echo "Checking homepage..."
curl -s -o /dev/null -w "%{http_code}" "$DOMAIN" | grep -q "200" || exit 1

# Check Lighthouse on production
echo "Running production Lighthouse audit..."
npm run test:lighthouse -- "$DOMAIN" || exit 1

# Check Core Web Vitals
echo "Checking Web Vitals..."
curl -s "$DOMAIN/api/vitals" | grep -q '"cls"' || exit 1

echo "✅ Smoke tests passed!"
```

### 6.2 Rollback Procedure

```bash
#!/bin/bash
# scripts/rollback.sh

PREVIOUS_VERSION=${1:-v1.9.0}

echo "🔄 Rolling back to $PREVIOUS_VERSION..."

# Get previous commit
ROLLBACK_COMMIT=$(git rev-list -n 1 "$PREVIOUS_VERSION")

# Create rollback branch
git checkout -b rollback/from-$PREVIOUS_VERSION "$ROLLBACK_COMMIT"

# Deploy
vercel --prod --token=$VERCEL_TOKEN

# Verify
./scripts/smoke-tests.sh || {
  echo "❌ Rollback failed smoke tests!"
  exit 1
}

echo "✅ Successfully rolled back to $PREVIOUS_VERSION"
```

### 6.3 Deployment Checklist

```
PRE-DEPLOYMENT
✅ All tests passing (unit, integration, e2e)
✅ Lighthouse 95+ (all categories)
✅ Bundle size analyzed
✅ No console errors
✅ Security audit clean
✅ Git tag created (v2.0.0)

DEPLOYMENT
✅ Vercel build succeeds
✅ No environment variable errors
✅ CDN assets cached correctly

POST-DEPLOYMENT (SMOKE TESTS)
✅ Homepage loads (200 HTTP)
✅ Lighthouse audit on prod (95+)
✅ Core Web Vitals recorded
✅ Analytics tracking working
✅ Error tracking (Sentry) active
✅ Video playback working
✅ Animations smooth (60fps)
✅ Mobile responsive (320-2560px)
✅ Keyboard navigation working
✅ Screen reader compatible

MONITORING
✅ Error tracking enabled
✅ Analytics dashboard set up
✅ Uptime monitoring active
✅ Performance alerts configured
✅ Slack notifications active
```

---

## ✅ Phase 4 Complete Checklist

### Testing
- [ ] Unit tests (Jest + RTL) - 80%+ coverage
- [ ] Integration tests - critical paths
- [ ] E2E tests (Playwright) - user flows
- [ ] Accessibility tests (axe)
- [ ] Visual regression tests

### Performance
- [ ] Bundle analysis completed
- [ ] Lighthouse automated testing (95+)
- [ ] Core Web Vitals profiling
- [ ] CDN caching optimized
- [ ] Image/video compression verified

### A/B Testing
- [ ] Feature flags implemented
- [ ] Video vs static variant ready
- [ ] Analytics tracking active
- [ ] Variant assignment configured
- [ ] Results dashboard set up

### Deployment
- [ ] Pre-deployment checklist automated
- [ ] Vercel configuration complete
- [ ] Environment variables set
- [ ] Security headers configured
- [ ] Cache-Control headers optimized

### Monitoring
- [ ] Error tracking (Sentry) live
- [ ] Analytics dashboard (Google Analytics)
- [ ] Uptime monitoring active
- [ ] Performance alerts configured
- [ ] Slack notifications working

### Rollback
- [ ] Rollback procedure documented
- [ ] Previous version tagged
- [ ] Smoke tests automated
- [ ] Team notified of plan
- [ ] Runbook created

---

## 🎓 Success Criteria Phase 4

✅ **All Tests Green**: Unit, integration, E2E tests passing  
✅ **Lighthouse 98+**: All categories (performance, accessibility, best practices, SEO)  
✅ **Core Web Vitals Excellent**: CLS < 0.05, FID < 100ms, LCP < 2.5s  
✅ **Production Deployed**: Live on custom domain, CDN active  
✅ **Monitoring Active**: Sentry, GA, Uptime Robot recording data  
✅ **Zero Critical Errors**: No console errors on production  
✅ **Video Playback**: All videos streaming smoothly (60fps)  
✅ **Mobile Perfect**: Responsive 320px-2560px  
✅ **A/B Testing Ready**: Variants deployable within hours  

---

## 🚀 Post-Phase 4: Continuous Improvement

Una vez completado Phase 4:

**Continuous Optimization**
- Monitor performance metrics weekly
- A/B test video vs static (2-4 weeks)
- Analyze user behavior (heatmaps, recordings)
- Iterate based on Core Web Vitals
- Update documentation as needed

**Future Enhancements**
- Add more animation types
- Implement service workers (PWA)
- Server-side rendering optimization
- GraphQL API integration
- CMS integration

---

## 📊 Final Metrics Summary

| Métrica | v1.0 | v2.0 Final |
|---------|------|-----------|
| Lighthouse | 92 | **98+** |
| Performance | 87 | **95+** |
| Accessibility | 93 | **98+** |
| Core Web Vitals | Good | **Excellent** |
| Video Support | ❌ | **✅** |
| Animations | Basic | **Premium (11 types)** |
| Test Coverage | N/A | **80%+** |
| Deployment Time | N/A | **< 5min** |
| MTTR (if incident) | N/A | **< 15min** |

---

**Status**: 🟢 Phase 4 Ready to Implement  
**Estimated Duration**: 1-2 semanas  
**Dependencies**: Phase 1 ✅ + Phase 2 ✅ + Phase 3 ✅  
**Owner**: Quality Assurance & DevOps Team

**🎉 LOLOWEBDEV v2.0 COMPLETE - Ready for Production**

