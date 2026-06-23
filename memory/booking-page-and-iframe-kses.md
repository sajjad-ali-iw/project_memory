---
name: booking-page-and-iframe-kses
description: Book a Free Appointment page (Google Calendar embed) + the KSES iframe-stripping gotcha for MCP/CLI-created pages
metadata: 
  node_type: memory
  type: project
  originSessionId: a85d9f1b-a80a-4009-9371-004be479765f
---

**`/book-a-free-appointment/` built 2026-06-23** (cloned from staging at user request). Page "Book a Free Appointment", **id 575**, blank `industry` template, self-contained page content (no theme file — MCP-managed model). Content = lavender `#ECE6FF` centered hero (H1 + subtitle, **no eyebrow** per [[no-eyebrow-in-hero]]) + a **Google Calendar Appointment Scheduling iframe** (`calendar.google.com/calendar/u/0/appointments/schedules/AcZssZ2W...`, width 100% / height 1300, radius 16). Staging's booking widget IS that Google Calendar embed, so the clone is **fully functional** (Google-hosted, points at the owner's calendar) — no form wiring needed (unlike the Contact form). This resolves the link-audit item: CTAs across the site link to `/book-a-free-appointment/`, which now exists.

**⚠️ GOTCHA — KSES strips `<iframe>` (and other embeds) when a page is saved without an admin user.** First `wp_insert_post` via CLI ran with no current user → `current_user_can('unfiltered_html')` false → WordPress KSES silently removed the iframe (page rendered with the hero but no calendar). **Fix:** `wp_set_current_user(<an administrator ID>)` BEFORE the insert/update so `unfiltered_html` is allowed; then the iframe is preserved. **This applies to ANY page created via MCP or CLI that contains an iframe/script/embed** — the MCP `wp_create_page` tool would strip it the same way unless it saves as an admin. Always verify the embed survived (`get_post(id)->post_content` contains the embed URL) after creating embed pages. Related: [[image-webp-media-library]], [[contact-about-pages]] (contact form still NOT wired).
