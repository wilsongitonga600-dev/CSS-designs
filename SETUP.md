# WEATHER // SYSTEM — Setup Guide

A futuristic HUD weather dashboard built with vanilla HTML, CSS, and JavaScript. This guide walks you through getting it running with live data.

---

## 1. Files

You have two options:

- **`weather-dashboard.html`** — single self-contained file (HTML + CSS + JS combined). Easiest to open, host, or sideload.
- **`index.html` / `style.css` / `script.js`** — the same app split into three files, if you prefer editing them separately.

Either version works identically. This guide uses the single-file version, but the steps are the same either way.

---

## 2. Get a free API key

The dashboard pulls live weather from **OpenWeatherMap**.

1. Go to **https://openweathermap.org/api**
2. Create a free account and confirm your email.
3. Go to **My API Keys** in your account dashboard.
4. Copy the key shown there (a long string of letters/numbers).

> ⏳ New keys can take up to **10–60 minutes** to activate. If you get errors right after signing up, wait a bit and try again.

### About the One Call API 3.0

This app uses OpenWeatherMap's **One Call API 3.0**, which bundles current weather, hourly forecast, daily forecast, and UV index into a single request.

- OpenWeatherMap's free tier includes **1,000 calls/day** to One Call 3.0 at no cost — you must still **subscribe** to it (still free) under **"One Call API 3.0"** on your account page, or requests will fail with a 401 error even with a valid key.
- If you'd rather not subscribe to One Call 3.0, see **Section 6 (Alternative: Free-tier-only API)** below for how to swap it for the older free endpoints.

---

## 3. Add your API key to the code

1. Open **`weather-dashboard.html`** in any text editor (Smart IDE, Termux + nano/vim, VS Code, etc.).
2. Find this block near the top of the `<script>` section (search for `API_KEY`):

   ```javascript
   const API_KEY = "YOUR_API_KEY"; // <-- PUT YOUR API KEY HERE
   ```

3. Replace `"YOUR_API_KEY"` with your real key, keeping the quotes:

   ```javascript
   const API_KEY = "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6";
   ```

4. Save the file.

That's the only required change to get live data working.

---

## 4. Run it locally

### Option A — Just open the file
Since everything is in one HTML file, you can usually just double-tap/double-click it to open it in your browser.

> ⚠️ Some browsers restrict `fetch()` requests when opening a file directly from disk (`file://`). If the dashboard gets stuck on **"SCANNING ATMOSPHERIC CONDITIONS..."** with no data ever loading, use Option B instead.

### Option B — Serve it locally (recommended)

**On Android via Termux:**

```bash
# Install Python if you don't have it
pkg install python

# Move into the folder containing weather-dashboard.html
cd /path/to/weather-dashboard

# Start a simple local server
python -m http.server 8000
```

Then open your phone's browser and go to:

```
http://localhost:8000/weather-dashboard.html
```

**On desktop (Node.js installed):**

```bash
npx serve .
```

Then open the URL it prints (usually `http://localhost:3000`).

---

## 5. Using the dashboard

- **SEARCH CITY... → SCAN** — type a city name and press the SCAN button (or hit Enter) to load weather for that location.
- **Location icon (target symbol)** — uses your device's GPS/browser geolocation to load weather for where you are. Your browser will ask for location permission the first time.
- **°C / °F button** — toggles temperature units across the whole dashboard.
- **Refresh icon** — reloads weather data for the currently displayed location.

On first load, the app automatically tries to fetch weather for **Nairobi** — change the default city by editing this line near the bottom of the script:

```javascript
loadWeatherForCity("Nairobi");
```

---

## 6. Alternative: free-tier-only API (no One Call 3.0 subscription)

If you don't want to subscribe to One Call 3.0, you can rebuild the fetch logic around OpenWeatherMap's always-free endpoints instead:

- Current weather: `https://api.openweathermap.org/data/2.5/weather`
- 5 day / 3 hour forecast: `https://api.openweathermap.org/data/2.5/forecast`

This requires combining two separate responses into the shape `normalizeWeatherData()` expects in `script.js`, and the UV Index metric panel will likely need to stay hidden since it isn't reliably available on the free tier. The `buildOneCallUrl()` function in `script.js` has comments marking where to make this swap.

---

## 7. Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Red **SYSTEM ERROR — NO API KEY CONFIGURED** on load | `API_KEY` still says `"YOUR_API_KEY"` | Add your real key (Section 3) |
| **SYSTEM ERROR** right after adding a fresh key | Key not activated yet | Wait 10–60 min after signup |
| **SYSTEM ERROR** even with an old, valid key | Not subscribed to One Call API 3.0 | Subscribe (free) on your OpenWeatherMap account page, or use Section 6 |
| **LOCATION NOT FOUND** | City name typo or very small/ambiguous place | Try a nearby larger city, or "City, Country" |
| **LOCATION ACCESS DENIED** | Browser location permission blocked | Re-enable location permission for the site in browser settings |
| Page loads but no data ever appears, no error shown | Opened via `file://` and `fetch()` was blocked | Serve locally instead (Section 4, Option B) |

---

## 8. Deploying it publicly (optional)

Since it's a single static HTML file with no build step, you can host it for free on any static host:

- **GitHub Pages** — push the file to a repo, enable Pages in repo settings.
- **Netlify / Vercel (drop deploy)** — drag and drop the file onto their web dashboard.
- **Cloudflare Pages** — same idea, connect a repo or drop the folder.

No server-side code or build process is required — it's plain HTML/CSS/JS.
