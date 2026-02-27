# ArmyVerse (Non‑Negotiable) — Scrapers & Cron

These answers are written strictly from what exists in the repo: `Documents/ARMYVERSE/`.

Relevant code I am referencing:
1. Spotify kworb scraper: `Documents/ARMYVERSE/lib/spotify/kworb.ts`
2. YouTube kworb scraper: `Documents/ARMYVERSE/lib/youtube/kworb.ts`
3. Spotify cron API: `Documents/ARMYVERSE/app/api/spotify/kworb/cron/route.ts`
4. YouTube cron API: `Documents/ARMYVERSE/app/api/youtube/kworb/cron/route.ts`
5. YouTube detail caching API: `Documents/ARMYVERSE/app/api/youtube/data/route.ts`
6. Homepage cache cron: `Documents/ARMYVERSE/app/api/trending/homepage-cache/cron/route.ts`
7. Cron docs: `Documents/ARMYVERSE/docs/api/cron.md` and `Documents/ARMYVERSE/docs/setup/cron-jobs.md`
8. Fandom scraper script + docs: `Documents/ARMYVERSE/scripts/scrape-fandom-galleries.js` and `Documents/ARMYVERSE/docs/scripts/fandom-gallery-scraper.md`

---

## 1) Explain your scraping pipeline.

### Short intro
In ArmyVerse, I scrape two main types of “trending/analytics” data from the web and store it in MongoDB so the UI can read it fast later.

1. kworb.net scraping for Spotify + YouTube analytics snapshots (daily).
2. BTS Fandom gallery scraping (bulk ingest) for the photocard catalog.

### Step-by-step explanation
1. **A cron trigger hits an internal API route**. Spotify uses `POST /api/spotify/kworb/cron` (`Documents/ARMYVERSE/app/api/spotify/kworb/cron/route.ts`) and YouTube uses `POST /api/youtube/kworb/cron` (`Documents/ARMYVERSE/app/api/youtube/kworb/cron/route.ts`).
2. **Cron route authenticates the request** using `CRON_SECRET` (details in question 3 below).
3. **Cron route connects to MongoDB** using `connect()` (Mongoose).
4. **The scraper fetches HTML pages**. Spotify scraping logic is in `Documents/ARMYVERSE/lib/spotify/kworb.ts`. YouTube scraping logic is in `Documents/ARMYVERSE/lib/youtube/kworb.ts` (it fetches `https://kworb.net/youtube/artist/bts.html`).
5. **HTML is parsed using Cheerio** (server-side HTML parser).
6. **Data is normalized into objects**. Spotify stores totals + per-song rows and also tries to extract Spotify IDs and fetch album art via Spotify oEmbed. YouTube stores title/views/yesterday/published, derives `videoId`, and generates thumbnail + watch URL.
7. **The snapshot is cached in MongoDB** using an upsert by `dateKey` (Spotify uses `KworbSnapshot.findOneAndUpdate(...)`, YouTube uses `YouTubeKworbSnapshot.findOneAndUpdate(...)`).
8. **Frontend reads the cached snapshot** using “latest” APIs, so normal page loads do not scrape live.
9. **Extra: YouTube video detail is fetched on-demand and cached**. The route `GET /api/youtube/data?videoId=...` in `Documents/ARMYVERSE/app/api/youtube/data/route.ts` caches detail inside today’s snapshot for ~24 hours, using `fetchVideoDetail()` from `Documents/ARMYVERSE/lib/youtube/kworb.ts`.
10. **Separate pipeline (bulk ingest): Fandom gallery scraper**. The script `Documents/ARMYVERSE/scripts/scrape-fandom-galleries.js` crawls `Category:Galleries`, visits gallery pages, extracts images + anchor/tab metadata, and upserts into `fandom_gallery_images` (with concurrency, delay, and retry/backoff).

### Design reasoning (why I did it)
1. Scraping is slow and sometimes unstable, so I don’t want to do it on every user request.
2. Storing daily snapshots in MongoDB gives me consistent performance for users.
3. Keeping scrapers in `lib/*` and cron triggers in `app/api/*` keeps the code explainable.

### Possible improvement (1–2 lines)
I would add retry/backoff for kworb scraping too (like the Fandom script), and add alerting when a scraper starts returning empty tables.

### Hard Terms Explained Simply
- `Upsert` = update if it exists, otherwise create.
- `Snapshot` = a saved copy of data for a given day.

---

## 2) What is Cheerio and how does it work?

### Short intro
Cheerio is a server-side tool that lets me query HTML like jQuery. I use it to read tables from scraped pages.

### Step-by-step explanation
1. I download HTML as a string using `fetch()`.
2. I load it with Cheerio: `const $ = cheerio.load(html)` (example in `Documents/ARMYVERSE/lib/spotify/kworb.ts` and `Documents/ARMYVERSE/lib/youtube/kworb.ts`).
3. Then I select elements like: “find the right table by checking header `<th>` text”, and “loop through `<tr>` rows and read `<td>` cell values”.
4. I clean numeric strings using a helper that strips commas and symbols (both scrapers have a `num()` helper).
5. I return clean objects, not raw HTML.

### Design reasoning (why I did it)
kworb is HTML-first. Cheerio is lightweight and good enough for parsing tables without running a real browser.

### Possible improvement (1–2 lines)
If the site starts using heavy client-side rendering, I’d switch to a headless browser approach for those pages only.

### Hard Terms Explained Simply
- `DOM` = the tree structure of an HTML page.
- `Selector` = a pattern like `table th` to pick elements from HTML.

---

## 3) What is a Cron job?

### Short intro
A cron job is a scheduled task. In my project it means “hit these internal endpoints on a schedule” so snapshots and caches stay fresh.

### Step-by-step explanation
1. Cron is configured outside the app (for example via Vercel cron or an external scheduler).
2. The scheduler calls an API endpoint at a fixed time.
3. That endpoint runs server-side code to refresh cached data.

Concrete examples in my codebase:
1. Vercel cron config exists in `Documents/ARMYVERSE/vercel.json`. It schedules `GET /api/trending/homepage-cache/cron` daily, and the handler is `Documents/ARMYVERSE/app/api/trending/homepage-cache/cron/route.ts`.
2. kworb cron endpoints are documented in `Documents/ARMYVERSE/docs/api/cron.md` and implemented in `Documents/ARMYVERSE/app/api/spotify/kworb/cron/route.ts` and `Documents/ARMYVERSE/app/api/youtube/kworb/cron/route.ts`.

### Design reasoning (why I did it)
This makes the app feel fast. Users always see cached data, and the heavy scraping happens only on schedule.

### Possible improvement (1–2 lines)
I would store a “last run status” record in DB and show it in an admin page for easy monitoring.

### Hard Terms Explained Simply
- `Cron schedule` = the time format like `0 2 * * *` meaning “every day at 2:00”.
- `Scheduler` = the service that triggers the job.

---

## 4) What happens if HTML structure changes?

### Short intro
If kworb/Fandom changes their HTML, my selectors can fail. Best case: I get partial data. Worst case: the snapshot becomes empty and the UI shows “no data yet”.

### Step-by-step explanation
1. Spotify kworb parsing tries to find the right table by checking header text like `song/streams/daily`. If that fails, it falls back to a heuristic “best table” scoring based on numeric-looking rows. This logic is in `Documents/ARMYVERSE/lib/spotify/kworb.ts`.
2. YouTube kworb parsing looks for a table with headers like `video/views/yesterday`. If it can’t find it, it warns and returns an empty list. This logic is in `Documents/ARMYVERSE/lib/youtube/kworb.ts`.
3. The Fandom gallery script expects specific gallery markup and tab/heading anchors. If those change, it may miss images or build wrong subcategory paths. This logic is in `Documents/ARMYVERSE/scripts/scrape-fandom-galleries.js`.
4. When any of these parsers fail, the cron route can still run but save partial/empty data. Then “latest” endpoints will return “no data available” until fixed.

### Design reasoning (why I did it)
Scraping is always brittle. For my project level, I focused on caching + graceful fallback instead of building a complex parser framework.

### Possible improvement (1–2 lines)
I would add “schema validation” for scraper outputs and alert when totals drop suddenly (example: 200 songs yesterday, 0 today).

### Hard Terms Explained Simply
- `Heuristic` = a smart guess approach when exact rules are not stable.
- `Validation` = checking the scraped output looks sane before saving it.

---

## 5) How do you avoid getting blocked?

### Short intro
I do basic “politeness + stability” things: set a real User-Agent, avoid caching, add delay, and use retries in the heavy crawler script.

### Step-by-step explanation
1. I use a desktop-like User-Agent in kworb scrapers (see `Documents/ARMYVERSE/lib/spotify/kworb.ts` and `Documents/ARMYVERSE/lib/youtube/kworb.ts`).
2. In the heavy crawler (Fandom script), I limit concurrency (default `3`), add delays (default `250ms`), and retry with backoff for `429` and `5xx` responses (see `Documents/ARMYVERSE/scripts/scrape-fandom-galleries.js`).
3. I cache results in MongoDB so users don’t hit the scraped site. Normal users read from snapshots, and only cron hits kworb.
4. I protect cron endpoints using a secret token (`CRON_SECRET`) unless dev bypass is enabled (see `Documents/ARMYVERSE/app/api/spotify/kworb/cron/route.ts` and `Documents/ARMYVERSE/app/api/youtube/kworb/cron/route.ts`).

### Design reasoning (why I did it)
This setup reduces scraping frequency and makes it less likely that kworb/Fandom will rate-limit the app.

### Possible improvement (1–2 lines)
I would add a shared “retry + backoff” wrapper for kworb requests too, and store the last scrape error in DB for quick debugging.

### Hard Terms Explained Simply
- `Rate limit` = the site blocks you if you send too many requests too fast.
- `Backoff` = wait longer before retrying when a request fails.
