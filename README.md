# AI Tools — LeadGen & PriceCheck

> Two zero-dependency, single-file web apps powered by the [Apify](https://apify.com) API. Open directly in any browser — no build step, no server, no npm.

**Live demo:** https://ibengwee.github.io/AI-/

---

## Apps

### LeadGen (`index.html`)
Find business leads using Google Maps data. Enter a sector, location, and optional job role — the app scrapes matching businesses and returns their name, category, phone, email, and website.

### PriceCheck (`price-finder.html`)
Find the cheapest price for any product across the web. Enter a product name, pick a country, and the app searches Google Shopping and returns results sorted from cheapest to most expensive with a trophy banner highlighting the best deal.

---

## Features

**LeadGen**
- Search businesses by sector, location, and role
- Returns name, category, email, phone, and website
- Dynamic table — adapts to any actor output schema
- Export results to CSV

**PriceCheck**
- Search any product across Google Shopping
- Results sorted cheapest first with trophy banner
- Sortable columns (price, store, rating)
- Country selector (US, UK, AU, CA, SG, MY, IN, DE, FR)
- Export results to CSV

**Both apps**
- Apify API key saved in `localStorage` (persists across sessions)
- Show/hide API key toggle
- Live status bar with animated pulse while polling
- Dark theme with purple accent
- Mobile responsive

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML, CSS, JavaScript (no framework) |
| Data | [Apify REST API](https://docs.apify.com/api/v2) — async run + polling |
| LeadGen actor | `compass~crawler-google-places` (Google Maps Scraper) |
| PriceCheck actor | `apify/google-shopping-scraper` |
| Hosting | GitHub Pages |
| Deployment | GitHub Actions (`peaceiris/actions-gh-pages`) |

---

## Setup

1. **Get an Apify API token** — sign up at [apify.com](https://apify.com) (free tier available)
2. **Open the app** — open `index.html` or `price-finder.html` directly in your browser
3. **Enter your token** — paste it into the API Key field (saved automatically for future visits)
4. **Search** — fill in the fields and click the button

No installation, no dependencies, no server required.

---

## Screenshots

> Add screenshots here after first run.

| LeadGen | PriceCheck |
|---------|-----------|
| _(screenshot)_ | _(screenshot)_ |

---

## Switching Apify Actors

To point an app at a different actor, edit the `<script>` section of the HTML file:

1. Change `ACTOR_ID` to the new actor's ID
2. Update the `body` inside `startRun()` to match the actor's input schema
3. Update `FIELD_MAP` aliases to match the actor's output field names

> **Note:** Some Apify actors require a paid plan when called via the REST API. If you see an error mentioning the free plan, switch to a different actor.

---

## Deployment

Pushes to `main` automatically deploy to GitHub Pages via the workflow in `.github/workflows/deploy.yml`.
