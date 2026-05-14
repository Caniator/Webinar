# Phase 1 — Stack & Architecture Decisions

**Project:** Aerones webinar landing page — *From Inspection to Insight: The Aerones Wind Operations Stack — Live*
**Date locked in:** June 11, 2026 · 3:00 PM CEST / 8:00 AM CT

---

## Core stack

| Layer | Choice | Notes |
|---|---|---|
| Webinar platform | **Zoom Webinar** | Requires Zoom Webinar paid add-on. Confirm license is active. |
| Lead capture / CRM | **Pipedrive** | Need admin access to set up custom fields + API token or webhook. |
| Hosting | **Vercel** | Free tier sufficient. Deploy from GitHub. Point `webinar.aerones.com` subdomain via DNS. |
| Integration middleware | **Zapier or Make** (recommended) | Make is cheaper for same job. Alternative: direct API calls from Vercel serverless function (more maintenance). |

---

## Integration architecture

On form submission, the lead must be:

1. **Created in Pipedrive** as a lead/contact with custom fields populated (role, country, company, webinar source).
2. **Registered in Zoom Webinar** via Zoom API so the join link is auto-sent by Zoom.
3. **Tagged for follow-up** — confirmation email, 24h reminder, post-webinar recording delivery.

**Flow:**

```
Landing page form
      ↓ (POST)
   Webhook (Zapier / Make)
      ├──→ Pipedrive (create lead + custom fields)
      └──→ Zoom Webinar (register attendee → Zoom sends join link)
```

---

## Pipedrive custom fields to create

- `Webinar Source` (text) — set to "Inspection to Insight – June 11 2026"
- `Role` (single option) — Wind farm operator / Asset manager / OEM service team / Consultant / Other
- `Country` (text or single option)
- `Job title` (text)

---

## Form fields (final)

- First name *
- Last name *
- Work email *
- Company *
- Job title *
- Country *
- Which best describes you? * → *Wind farm operator / Asset manager / OEM service team / Consultant / Other*

---

## Prerequisites checklist (resolve before Phase 5)

- [ ] Zoom Webinar license confirmed active
- [ ] Pipedrive admin access available; custom fields created
- [ ] Zapier or Make account created (Make has more generous free tier)
- [ ] DNS access for `webinar.aerones.com` (or chosen subdomain)
- [ ] Vercel account created and connected to GitHub

---

## Tracking & analytics

- Google Analytics 4 (or Plausible) — track: `page_view`, `cta_click`, `form_submit`
- Confirmation email triggered on Zapier/Make webhook fan-out
- Add to calendar links on thank-you page (Google / Outlook / iCal)
