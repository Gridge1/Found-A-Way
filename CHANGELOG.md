# Found A Way — Changelog

All changes documented here in reverse chronological order.
Format: **[vX.X]** — Date — What changed and why.

---

## [v1.0] — 2025-05-25 — Search Form

Built the complete v1 UI in `index.html`. No backend required — opens directly in a browser.

**What's in v1:**
- Search form: origin + destination with airport autocomplete (200+ airports worldwide)
- Return / one-way toggle
- Outbound and return date pickers (minimum date enforced)
- Flexible ±3 days checkbox
- Passenger selector (adults, children, infants) with popup counter
- Cabin class dropdown (Economy / Premium Economy / Business / First)
- "Compare prices" → Google Flights deeplink
- "Book now" → Skyscanner deeplink (affiliate placeholder)
- Multi-airport panel: when a country is selected as origin, shows one Skyscanner link per airport in that country (capped at 100)
- Info panels (visa, vaccinations, currency, airports, booking window, hotels) appear when destination is selected — placeholder state, data coming in v2
- Mobile-first responsive design
- Deep navy + amber colour scheme
- Footer with affiliate disclosure and gov.uk health disclaimer

**Snapshot saved:** `versions/v1.0-search-form.html`

**Next:** v2 — live currency rates, vaccination data, airport info (requires Netlify Functions + API keys)

---

## [v0.1] — 2025-05-25 — Project Foundation

Created all project foundation files. No UI built yet.

**Files created:**
- `CLAUDE.md` — Rules for every Claude Code session
- `SPEC.md` — Full technical specification and version roadmap
- `CHANGELOG.md` — This file
- `README.md` — Developer setup guide
- `data/visa-fallback.json` — Visa requirements for 28 countries (UK passport holders)
- `data/vaccinations.json` — Vaccination data for 28 countries
- `data/booking-windows.json` — Booking windows for key routes and 8 regions
- `data/airports.json` — Details for 10 UK departure airports

**Next step:** Build v1 — search form + Google Flights / Skyscanner links.
