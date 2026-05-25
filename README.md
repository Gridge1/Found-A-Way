# Found A Way

A public PWA travel companion. Gives you everything you need before booking a flight — visa info, vaccinations, live currency, airport details, and the best time to book. Earns through affiliate links.

## Tech Stack

| Layer | Tool |
|---|---|
| Frontend | Vanilla HTML/CSS/JS — single `index.html` |
| Hosting | Netlify |
| Serverless functions | Netlify Functions (Node.js) |
| Database | Supabase |
| Emails | Resend |
| Scheduled jobs | GitHub Actions |

## Local Development

**v1 (static only):** Just open `index.html` in a browser. No server needed.

**v2+ (with Netlify Functions):**
1. Install Netlify CLI: `npm install -g netlify-cli`
2. Log in: `netlify login`
3. Run locally: `netlify dev`
4. Open: `http://localhost:8888`

## Environment Variables

Set these in Netlify dashboard → Site Settings → Environment Variables. Never put them in code.

```
EXCHANGERATE_API_KEY=        # From exchangerate-api.com (free tier)
AERODATABOX_API_KEY=         # From RapidAPI (free tier)
RESEND_API_KEY=              # From resend.com (free tier)
SUPABASE_URL=                # From supabase.com project settings
SUPABASE_ANON_KEY=           # From supabase.com project settings
SKYSCANNER_ID=               # Skyscanner affiliate ID
BOOKING_ID=                  # Booking.com affiliate partner ID
IVISA_ID=                    # iVisa affiliate ID
SAFETYWING_ID=               # SafetyWing affiliate ID
GETYOURGUIDE_ID=             # GetYourGuide affiliate ID
```

## Data Files

Static JSON files in `/data/`. Updated manually or by GitHub Action scraper (v8).
All shown in the UI with a "last updated" date and a "verify before travel" disclaimer.

| File | What it contains |
|---|---|
| `visa-fallback.json` | Visa requirements for 28 countries (UK passport holders) |
| `vaccinations.json` | Vaccination requirements and recommendations per country |
| `booking-windows.json` | Best weeks to book per route and region |
| `airports.json` | Terminal, lounge, and facility info for UK departure airports |

## Version Snapshots

After each build session, a snapshot of `index.html` is saved to `/versions/`.
Full history in `CHANGELOG.md`.

## Rules

See `CLAUDE.md` — strict one-feature-at-a-time process.
See `SPEC.md` — full roadmap and technical decisions.
