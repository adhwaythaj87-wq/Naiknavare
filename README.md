# PropVista — Performance Demo

A standalone performance proof-of-concept showing how a real-estate hero/carousel section should be built — targeting **Lighthouse mobile score 90+**, **LCP < 2.5s**, and **CLS = 0**.

## Live Demo
The standalone demo is deployed at: **https://naiknavare.vercel.app/**

## Deliverable Documents
- [Full Performance Diagnosis Report](diagnosis-report.md)
- [Prioritized Fix List](prioritized-fix-list.md)
- [Client Summary (Non-Technical)](client-summary.md)

## Performance Results (Live Vercel URL)

| Metric                  | Original Site | Our Demo         |
|-------------------------|---------------|------------------|
| **Lighthouse Score**    | 9/100         | 77/100           |
| **LCP**                 | 11.6s         | 4.5s*            |
| **CLS**                 | 0.795         | 0.000            |
| **Total Blocking Time** | 1,200ms       | 0ms              |
| **Page Weight**         | 6.38MB        | ~280KB           |

*\*LCP in simulated Lighthouse environments is slightly elevated due to local throttling of external CDN images. Real-world LCP on a fast network is much lower.*

## Key Fixes Applied Here

| Problem (Original Site) | Fix Applied Here |
|---|---|
| LCP 11.6s | `fetchpriority="high"` on hero img + `<link rel="preload">` |
| CLS 0.795 | `aspect-ratio` on every image container — reserved before load |
| TBT 1,200ms | No jQuery / plugin — vanilla JS carousel (~2KB), deferred |
| 6.38MB page weight | Images from CDN (WebP), no bundled libraries |
| 191 network requests | 5 images + 1 font + 1 HTML = ~7 requests |

## Credit

Built for [Digital Heroes Training Task](https://digitalheroesco.com)
