---
name: litespeed-purge-and-edge-cache
description: "The purge method that actually works, and the stale-edge-cache incident that must be understood before production cutover"
metadata:
  node_type: memory
  type: project
---

**Purge All from code: use the admin-bar node.** On any wp-admin page as a logged-in admin, `document.querySelector('#wp-admin-bar-litespeed-purge-all a')` and fetch its href (same-origin) — returns 200 and purges. Scraping the dashboard HTML for a `purge_all` link 404s (stale/wrong link in markup); stop doing that. Reminder from earlier lessons: LiteSpeed also caches 404s, so purge AGAIN after creating a page on a brand-new slug.

**⚠️ Cutover-critical incident (2026-07-30):** a guest fetch of `/multi-channel-ecommerce/` served a **July-25 copy (old H1 + old title) that had survived multiple Purge Alls** across several days — while WordPress raw content and Yoast were verified current. Only one network path served the stale copy (a sandbox/proxy egress); the user's browser path was fresh, and the next purge cycle finally cleared it. Best explanation: an edge layer (QUIC.cloud CDN or an intermediate proxy) holding its own cache that origin Purge All does not always reach. **Before production cutover someone must confirm:** is QUIC.cloud (or any CDN) enabled in LiteSpeed? If yes, cutover purges must ALSO purge the CDN zone, and the launch checklist must verify a changed page from an outside network, not just the office browser. A launch where purges don't reach the edge = stale pages for days while everyone believes the site is live.

Related: [[blog-bold-link-color-gotcha]], [[booking-page-and-iframe-kses]].
