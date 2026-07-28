# Prioritized Fix List: Naiknavare.com

Based on the performance diagnosis of the original site, the following recommendations are ranked by their potential impact vs. implementation effort.

## 🔴 High Impact, Low/Medium Effort (Do This First)

### 1. Reserve Space for Hero Images (Fix CLS)
- **Impact:** HIGH (Eliminates layout shifts, instantly improving the CLS score from 0.795 to 0).
- **Effort:** LOW
- **Action:** Add CSS `aspect-ratio` to the hero carousel container and `width`/`height` attributes to the `<img>` tags. This tells the browser exactly how much space to reserve before the heavy images finish downloading.

### 2. Implement Native Lazy Loading
- **Impact:** HIGH (Drastically reduces initial page weight and network congestion).
- **Effort:** LOW
- **Action:** Add `loading="lazy"` to all images that are below the fold (including slides 2-5 of the carousel). This ensures they do not compete with the critical first slide for bandwidth.

### 3. Preload the LCP Image
- **Impact:** HIGH (Improves Largest Contentful Paint significantly).
- **Effort:** LOW
- **Action:** Add `<link rel="preload" as="image" href="...">` in the `<head>` and `fetchpriority="high"` on the first hero image. This forces the browser to fetch the LCP asset immediately rather than discovering it late in the render cycle.

## 🟡 Medium Impact, Medium Effort

### 4. Remove jQuery and Heavy Carousel Plugins
- **Impact:** HIGH (Drastically reduces TBT from 1,200ms to near 0ms).
- **Effort:** MEDIUM
- **Action:** Replace the bloated, main-thread-blocking JavaScript carousel with a Vanilla JS and CSS-driven alternative. By using CSS transitions for the animation and `IntersectionObserver` for logic, you eliminate the massive JS execution cost on mobile devices.

### 5. Optimize Image Formats (WebP)
- **Impact:** MEDIUM (Reduces page weight by ~60% for image assets).
- **Effort:** MEDIUM
- **Action:** Convert heavy JPEG/PNG assets to WebP format. This can often be automated via a CDN or a build step.

## ⚪ Low Return on Investment (Skip for Now)

### 6. Full Single-Page Application (SPA) Rewrite
- **Impact:** HIGH
- **Effort:** VERY HIGH
- **Why it's not worth it:** Converting the entire multi-page marketing site to React/Next.js or Vue just for performance is massive overkill. The bottleneck isn't the server rendering; it's the unoptimized front-end assets. Fixing the CSS/JS and image tags as outlined above will yield a 90+ score at a fraction of the cost and risk.

### 7. Micro-optimizing CSS Selectors
- **Impact:** VERY LOW
- **Effort:** HIGH
- **Why it's not worth it:** Refactoring CSS to remove slightly inefficient selectors will not move the needle on this site. The style evaluation time is dwarfed by the heavy script execution and network bottlenecks.
