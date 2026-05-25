# Found A Way — Claude Code Rules

Read this file at the start of every session, without exception. Then read SPEC.md.

## What This Project Is
Found A Way is a public PWA travel companion. It wraps around flight search with practical pre-booking information: visa requirements, vaccinations, live currency rates, airport details, and best booking timing. Revenue comes entirely from affiliate links — there is no user subscription or data selling.

## Who You Are Working With
Alex is a beginner coder. Explain every significant change in plain English as you make it — what it does, why it's needed, and how it fits in. Never assume knowledge of web development concepts without explaining them first. Keep explanations concise but clear.

---

## File Rules
- All UI lives in `index.html` — edit it in place, never rewrite it from scratch
- After every session: save a snapshot to `/versions/vX.X-description.html`
- After every session: update `CHANGELOG.md` with what changed and why
- Never put API keys, secrets, or affiliate IDs directly in any file
- Netlify environment variables only — see SPEC.md for the full list

## Affiliate ID Placeholders
Use these exact strings until Alex confirms real IDs:

| Partner | Placeholder |
|---|---|
| Skyscanner | `YOUR_SKYSCANNER_ID` |
| Booking.com | `YOUR_BOOKING_ID` |
| iVisa | `YOUR_IVISA_ID` |
| SafetyWing | `YOUR_SAFETYWING_ID` |
| GetYourGuide | `YOUR_GETYOURGUIDE_ID` |

---

## One Feature At A Time
Complete one feature fully before starting the next. Confirm with Alex before moving on. Do not add extras that weren't asked for.

## Cost Target
Free tiers only unless Alex explicitly approves spending. See SPEC.md → API & Cost section. The Sherpa API is enterprise-priced — do not use it. Use `/data/visa-fallback.json` instead.

---

## Design Principles
- Mobile-first — design for 375px wide screens, then scale up
- Clean, modern, trustworthy — calm confidence, like a well-travelled friend
- Not garish, not budget-airline-website
- Target user: savvy independent traveller, solo/budget lean, wants everything in one place before booking. Think "informed" not "cheap".

---

## Current Status
Check `CHANGELOG.md` to see what has been built. Check `SPEC.md` for the full roadmap.
