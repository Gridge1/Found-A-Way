# Found A Way — Technical Specification

## Overview
A public progressive web app (PWA) giving travellers everything they need before booking a flight. Visa requirements, vaccinations, live currency, airport info, and the best time to book — all in one place. Earns through affiliate links. Free to use.

## Target User
Smart, independent traveller — solo or budget-lean, but "savvy" not "cheap". Booking a trip somewhere unfamiliar. Wants to open one tab instead of twelve. Skews younger, comfortable on mobile.

---

## Monetisation

| What triggers it | Partner | Type |
|---|---|---|
| "Compare prices" button | Google Flights | No affiliate — just useful |
| "Book now" button | Skyscanner | **Primary affiliate revenue** |
| Visa required → help applying | iVisa | Affiliate |
| Hotels tab | Booking.com | Affiliate |
| Insurance tab | SafetyWing | Affiliate |
| Activities tab | GetYourGuide | Affiliate |

Affiliate disclosure shown on every page (legal requirement).

---

## Tech Stack

### Frontend
- Vanilla HTML / CSS / JS — single `index.html` file
- No frameworks (keeps it fast, simple, and beginner-readable)
- CSS custom properties for theming

### Deployment
Manual drag-and-drop to Netlify dashboard. GitHub repo (github.com/Gridge1/Found-A-Way) is for version control only — not connected to Netlify CI.

## Infrastructure

| Service | Purpose | Free Tier |
|---|---|---|
| Netlify | Static hosting + serverless functions | 125k function calls/month, 100 GB bandwidth |
| Supabase | Database (price alerts, email storage) | 500 MB storage, 50k rows |
| GitHub Actions | Scheduled scraper jobs | Unlimited on public repos |
| Resend | Transactional email (price alerts) | 3,000 emails/month, 100/day |

### APIs & Cost

| API | Purpose | Free Tier | Strategy |
|---|---|---|---|
| ExchangeRate-API | Live GBP rates | 1,500 req/month | Fetch once daily, cache in localStorage |
| AeroDataBox (RapidAPI) | Airport details | 500 req/month | Cache all responses in localStorage |
| Sherpa | Visa data | **Not free — skip** | Use `/data/visa-fallback.json` |

### Static Data Files (no API cost)
| File | Contents |
|---|---|
| `/data/visa-fallback.json` | Visa rules per country for UK passport holders |
| `/data/vaccinations.json` | Vaccination requirements and recommendations per country |
| `/data/booking-windows.json` | Best booking timing per route and region |
| `/data/airports.json` | UK departure airport details (terminals, lounges, transit times) |

---

## Search Form Spec

### Origin
- Dropdown of UK airports: LHR, LGW, STN, LTN, LCY, EMA, MAN, EDI, BHX, BRS (and others)
- "Any UK airport" option — shows country picker (UK pre-selected) → all airports in that country shown, user can multi-select which ones to search
- Default: LHR (London Heathrow)

### Destination
- Country picker (shows flag + country name)
- Or specific airport (IATA code autocomplete)
- Default: blank

### Dates
- Outbound date picker
- Return date picker (optional — toggle for one-way)
- Flexible dates toggle (±3 days)

### Passengers
- Adults (18+)
- Children (2–17, with age selector per child for fare calculation)
- Infants (under 2)

### Other
- Cabin class: Economy / Premium Economy / Business / First
- Budget cap (optional, GBP per person)

---

## Version Roadmap

### v1 — Search Form + Flight Links
- Fully functional search form (UI only)
- "Compare prices" → Google Flights deeplink
- "Book now" → Skyscanner deeplink (affiliate placeholder)
- Clean mobile-first design
- **No backend needed for this version**

### v2 — Information Layer
- Currency tab: live GBP → destination rate, 30-day graph, simple calculator
- Vaccinations tab: required + recommended jabs, malaria risk level, source from `/data/vaccinations.json`
- Airport info tab: covers every airport in the journey (departure, layovers, arrival)
  - Terminals and which airlines use them
  - Lounges and how to access them
  - Key facilities: wifi, food, showers, pharmacy
  - Recommended transit time
  - Layover rating: 🔴 under 90 min (risky) | 🟡 90–180 min (tight) | 🟢 over 180 min (comfortable)
- Netlify Function: `/netlify/functions/currency.js` (keeps ExchangeRate-API key server-side)
- Netlify Function: `/netlify/functions/airports.js` (keeps AeroDataBox key server-side)

### v3 — Visa Info + Real Affiliate Links
- Visa tab: requirement type, max stay, cost, processing time
- iVisa affiliate link when visa/ETA/e-visa is required
- All affiliate links go live (Skyscanner, Booking.com, iVisa)
- Hotels card → Booking.com affiliate
- Insurance card → SafetyWing affiliate
- Activities card → GetYourGuide affiliate
- Legal pages: Privacy Policy, Terms of Use, Cookie Consent banner, Affiliate Disclosure

### v4 — PWA (Installable App)
- `manifest.json` — makes app installable on phone home screen
- Service worker (`sw.js`) — caches visa and vaccination data for offline use
- App icon set (multiple sizes for different devices)

### v5 — Price Alerts
- Email capture: "Alert me when prices drop to £X"
- Supabase stores: route, email, target price, alert frequency
- GitHub Action runs daily: checks Skyscanner prices, compares to targets
- Resend sends alert email with unsubscribe link
- GDPR-compliant: explicit consent, easy unsubscribe

### v6 — Booking Windows + Layover Ratings
- "Best time to book" card from `/data/booking-windows.json`
- Cheap month calendar heatmap
- Full layover rating system (live connection times from AeroDataBox)

### v7 — Extended Affiliates
- Booking.com hotel widget (destination-aware, dates pre-filled)
- SafetyWing quote with trip duration pre-filled
- GetYourGuide top activities for destination

### v8 — Auto Vaccination Scraper
- GitHub Action runs weekly
- Scrapes NHS Fit for Travel per country
- Updates `/data/vaccinations.json`
- Sends failure alert email (Resend) if scrape breaks

### v9 — Domain, SEO, Launch
- Custom domain (to be confirmed)
- Meta tags, Open Graph cards (for social sharing)
- Sitemap
- Google Search Console verification
- Privacy-friendly analytics (Plausible or Fathom — both have free tiers)

---

## Deeplink Formats

### Google Flights
```
https://www.google.com/travel/flights?q=Flights+from+{IATA_FROM}+to+{IATA_TO}+on+{YYYY-MM-DD}
```

### Skyscanner (affiliate)
```
https://www.skyscanner.net/transport/flights/{from}/{to}/{YYMMDD}/?adults={n}&ref=YOUR_SKYSCANNER_ID
```

### Booking.com (affiliate)
```
https://www.booking.com/searchresults.html?ss={city}&checkin={YYYY-MM-DD}&checkout={YYYY-MM-DD}&aid=YOUR_BOOKING_ID
```

### iVisa (affiliate)
```
https://www.ivisa.com/apply?country={ISO2}&ref=YOUR_IVISA_ID
```

---

## Netlify Environment Variables

Set these in: Netlify dashboard → Site Settings → Environment Variables

```
EXCHANGERATE_API_KEY=
AERODATABOX_API_KEY=
RESEND_API_KEY=
SUPABASE_URL=
SUPABASE_ANON_KEY=
SKYSCANNER_ID=
BOOKING_ID=
IVISA_ID=
SAFETYWING_ID=
GETYOURGUIDE_ID=
```

---

## Legal Requirements
- Cookie consent banner on first visit (GDPR)
- Privacy Policy page (what data is stored, how long, who sees it)
- Terms of Use page
- Affiliate Disclosure: "We may earn a commission when you book through our links — at no extra cost to you"
- Visa disclaimer: "Always verify requirements at gov.uk/foreign-travel-advice before travel"
- Health disclaimer: "Consult your GP or travel clinic before travel. This is general guidance only."
- Both disclaimers show last-updated date from the relevant JSON file

---

## File Structure
```
Found-A-Way/
├── index.html                   # All UI — edit in place
├── CLAUDE.md                    # AI rules — read every session
├── SPEC.md                      # This file
├── CHANGELOG.md                 # Version history
├── README.md                    # Developer setup
├── manifest.json                # PWA manifest (v4)
├── sw.js                        # Service worker (v4)
├── netlify/
│   └── functions/
│       ├── currency.js          # ExchangeRate-API proxy (v2)
│       ├── airports.js          # AeroDataBox proxy (v2)
│       └── price-check.js       # Price alert checker (v5)
├── data/
│   ├── visa-fallback.json       # Visa data (UK passports)
│   ├── vaccinations.json        # Vaccination data
│   ├── booking-windows.json     # Best booking timing
│   └── airports.json            # UK airport details
└── versions/
    └── v1.0-search-form.html    # Snapshot after v1 session
```
