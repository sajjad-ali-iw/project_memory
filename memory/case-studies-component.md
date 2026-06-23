---
name: case-studies-component
description: Case Studies carousel is its own section after the hero — not part of the hero
metadata: 
  node_type: memory
  type: project
  originSessionId: fc65fed4-f70a-454e-8557-0e1b35d9522e
---

The cards that used to live inside the hero are **case studies** and were split into their own component (user: "the cart is of case studies need to create a separate component... design is not right for card"). Built to match staging.indexworld.net.

- `patterns/case-studies.php` — slug `indexworld/case-studies`. White intro card (`.ix-cases__intro`: "Case Studies" eyebrow + "Proven Results Across Industries" title + lavender `.ix-cases__explore` pill + dark-navy `.ix-cases__round` arrow) overlapping a `.ix-cases__track` of 9 portrait cards from `assets/images/hero-cards/Frame-*.webp`.
- `assets/js/case-studies.js` — arrows + dynamically-built pagination dots; enqueued in `functions.php`.
- `.ix-cases` CSS in `components.css`: track has `padding-left:360px` to clear the absolute intro card; active dot = dark elongated pill `#00053D`; arrows = `#ECE6FF` circles. Mobile (≤900px): intro card goes static/full-width, track padding removed.
- Wired into `templates/front-page.html` **right after `indexworld/hero`**, before services.
- `hero.php` was rewritten to drop the carousel (class `ix-hero--noscroll`).

Verified rendering on local. Old `hero-carousel.js` enqueue is now unused (cleanup candidate). Related: [[hero-and-frontpage]], [[section-port-method]], [[frontpage-db-template-override]].
