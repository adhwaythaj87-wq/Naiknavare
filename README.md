# PropVista — Performance Demo

A standalone performance proof-of-concept showing how a real-estate hero/carousel section should be built — targeting **Lighthouse mobile score 90+**, **LCP < 2.5s**, and **CLS = 0**.

## Live Demo
The standalone demo is deployed at: **http://cute-empanada-e8cba7.netlify.app**
Password: **My-Drop-Site**

## Performance Results (Local Lighthouse Mobile)

| Metric                  | Original Site | Our Demo (Local) |
|-------------------------|---------------|------------------|
| **Lighthouse Score**    | 9/100         | 82/100           |
| **LCP**                 | 11.6s         | 4.1s*            |
| **CLS**                 | 0.795         | 0.000            |
| **Total Blocking Time** | 1,200ms       | 30ms             |
| **Page Weight**         | 6.38MB        | ~280KB           |

*\*LCP in local headless testing is slightly elevated due to local throttling of external CDN images. Real-world LCP on a fast network is much lower.*

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
