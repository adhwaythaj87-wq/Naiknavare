# Performance Diagnosis Report: Naiknavare Developers (naiknavare.com)

## Overview
This report details the web performance bottlenecks identified on the original Naiknavare home page (hero/carousel section) compared against a standalone, performance-optimized rebuild of the same component. 

*Note: The data presented here is "lab data" (simulated via Lighthouse). Field/CrUX data (Chrome User Experience Report) is currently unavailable, which is typical for smaller or regional websites that do not meet the minimum traffic thresholds required by Google to surface public field data.*

## Executive Summary
The original implementation suffers from severe rendering delays and layout instability, primarily caused by render-blocking JavaScript, heavy dependencies, and unoptimized image loading strategies. 

By applying modern performance primitives (native lazy loading, explicit aspect ratios, and vanilla JS), the rebuilt demo reduced the Largest Contentful Paint (LCP) and Total Blocking Time (TBT) dramatically, while achieving zero Cumulative Layout Shift (CLS).

## Metric Comparison

| Metric | Original Site | Optimized Rebuild | Status |
| :--- | :--- | :--- | :--- |
| **Lighthouse Performance Score** | 9 / 100 | 77 / 100 | 🟢 Improved |
| **LCP (Largest Contentful Paint)** | 11.6s | 4.5s | 🟢 Improved |
| **CLS (Cumulative Layout Shift)** | 0.795 | 0.000 | 🟢 Improved (Perfect) |
| **TBT (Total Blocking Time)** | 1,200ms | 0ms | 🟢 Improved |
| **Page Weight** | 6.38MB | ~280KB | 🟢 Improved |
| **Total Requests** | 191 | ~7 | 🟢 Improved |

## Root Cause Analysis & Bottlenecks

### 1. Main Thread Blocking (TBT: 1,200ms)
**What:** The browser's main thread is locked up for over a full second, preventing the page from becoming interactive.
**Why:** The original site loads multiple unminified JavaScript files, a heavy jQuery dependency, and a bloated carousel plugin directly in the critical render path. The browser must download, parse, compile, and execute these massive scripts before it can finish painting the page.

### 2. Excessive Layout Shifts (CLS: 0.795)
**What:** The page content jumps around visually as it loads, leading to a poor user experience and a failing Core Web Vitals score.
**Why:** Images in the original carousel do not have reserved space (no `width`/`height` attributes and no CSS `aspect-ratio`). When the heavy image files finally download, the browser is forced to abruptly recalculate the layout and push surrounding content down.

### 3. Delayed Largest Contentful Paint (LCP: 11.6s)
**What:** The main hero image takes nearly 12 seconds to fully render on mobile connections.
**Why:** The hero image is not prioritized. It competes with 190 other network requests (including render-blocking scripts and CSS) for bandwidth. There is no `<link rel="preload">` or `fetchpriority="high"` hint to tell the browser to fetch this critical asset immediately.

### 4. Network Payload & Page Weight (6.38MB, 191 Requests)
**What:** The user is forced to download over 6MB of data across nearly 200 separate requests just to see the top of the homepage.
**Why:** Images are oversized, unoptimized, and lack modern formats (like WebP). Furthermore, off-screen images (such as subsequent slides in the carousel) are loaded immediately rather than using native `loading="lazy"`. The excessive request count is heavily inflated by chained font requests and unbundled assets.
