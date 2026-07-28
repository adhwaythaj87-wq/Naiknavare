# PropVista — Performance Demo

A standalone performance proof-of-concept showing how a real-estate hero/carousel section should be built — targeting **Lighthouse mobile score 90+**, **LCP < 2.5s**, and **CLS = 0**.

## What it fixes

| Problem (Original Site) | Fix Applied Here |
|---|---|
| LCP 11.6s | `fetchpriority="high"` on hero img + `<link rel="preload">` |
| CLS 0.795 | `aspect-ratio` on every image container — reserved before load |
| TBT 1,200ms | No jQuery / plugin — vanilla JS carousel (~2KB), deferred |
| 6.38MB page weight | Images from CDN (WebP), no bundled libraries |
| 191 network requests | 5 images + 1 font + 1 HTML = ~7 requests |

## Credit

Built for [Digital Heroes Training Task](https://digitalheroesco.com)
