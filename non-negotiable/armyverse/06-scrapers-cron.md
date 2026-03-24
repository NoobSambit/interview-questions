# ArmyVerse (Non-Negotiable) - Scrapers & Cron

These answers are based on the implemented ArmyVerse project, but rewritten in simple interview language so they are easier to remember and explain.

---

## 1) Explain your scraping pipeline.

### Short intro
ArmyVerse uses scraping mainly for trending and analytics-style data that is too slow or unstable to fetch live for every user request. So the system scrapes, normalizes, stores snapshots in MongoDB, and then lets the UI read the cached result.

### Step-by-step explanation
1. A scheduled trigger calls an internal cron API route.
2. The cron route first verifies that the request is allowed.
3. Then it connects to MongoDB.
4. The scraper fetches the target HTML page, like the kworb Spotify or YouTube pages.
5. It parses the HTML and extracts the useful rows and values.
6. The data is cleaned and normalized into a consistent object shape.
7. The system stores that result in MongoDB as a daily snapshot, usually using an upsert pattern.
8. The frontend later reads the latest stored snapshot instead of scraping live during page load.
9. There is also a separate Fandom gallery ingestion flow that crawls and stores image data for the photocard system.

### Design reasoning
Scraping is slow and can break unexpectedly, so I did not want regular users to depend on live scraping. Storing snapshots gives better speed and more stable UI behavior.

### Possible improvement
I would add more alerting and stronger retry-backoff support for the kworb scrapers too.

### Hard Terms Explained Simply
- `Snapshot` = a saved copy of data for a specific time.
- `Upsert` = update if it exists, otherwise create a new record.
- `Normalize` = convert raw scraped data into one clean consistent structure.

---

## 2) What is Cheerio and how does it work?

### Short intro
Cheerio is a server-side HTML parser that feels similar to jQuery. I use it to read and navigate scraped HTML without needing a full browser.

### Step-by-step explanation
1. The scraper downloads the page HTML as a plain string.
2. Cheerio loads that HTML into a queryable structure.
3. Then I can use selectors to find tables, rows, cells, headings, and links.
4. After selecting the right elements, I read the text values I need.
5. Numeric strings are cleaned so they become proper numbers instead of messy formatted text.
6. The scraper finally returns clean JavaScript objects instead of raw HTML.

### Design reasoning
Cheerio is lightweight and works well for static HTML pages like ranking tables. It is much simpler than launching a real browser when that is not needed.

### Possible improvement
If a target site becomes heavily client-rendered, I would use a headless browser for only those pages.

### Hard Terms Explained Simply
- `Selector` = a pattern used to find elements in HTML.
- `DOM` = the tree-like structure of an HTML page.
- `Headless browser` = a browser that runs in code without opening a visible window.

---

## 3) What is a Cron job?

### Short intro
A cron job is a scheduled task. In ArmyVerse, cron means certain internal endpoints are triggered automatically at fixed times so cached data stays fresh.

### Step-by-step explanation
1. A scheduler outside the app decides when the job should run.
2. At that time, it calls one of the internal cron endpoints.
3. The endpoint runs server-side logic like refreshing snapshots or homepage caches.
4. The new data is stored in MongoDB.
5. When users later open the app, they read the already prepared data instead of waiting for the heavy job to run live.

### Design reasoning
This makes the user experience much faster and more stable, because scheduled work happens in the background instead of inside normal user requests.

### Possible improvement
I would store detailed last-run status in the database and expose it in an internal admin view.

### Hard Terms Explained Simply
- `Cron job` = a task that runs automatically on a schedule.
- `Scheduler` = the system that triggers the job.
- `Cache` = stored data reused later so users get faster responses.

---

## 4) What happens if HTML structure changes?

### Short intro
If the target site's HTML changes, the scraper can fail partially or fully because selectors and assumptions no longer match the page structure.

### Step-by-step explanation
1. The scraper depends on expected tables, headers, and row formats.
2. If the site changes those structures, the parser may not find the right data anymore.
3. In some cases, the scraper can still return partial data through fallback logic.
4. In worse cases, it may save an empty or incomplete snapshot.
5. Then the frontend will show missing or outdated data until the scraper is fixed.

### Design reasoning
Scraping is always brittle because we do not control the target site's markup. For this project, I focused on graceful failure and caching rather than building a very heavy parser framework.

### Possible improvement
I would add validation checks that compare today's scrape quality against expected ranges and alert when results suddenly collapse.

### Hard Terms Explained Simply
- `Brittle` = easy to break if something small changes.
- `Heuristic` = a practical smart guess used when exact rules are unreliable.
- `Validation` = checking whether the scraped result looks reasonable before trusting it.

---

## 5) How do you avoid getting blocked?

### Short intro
I reduce the chance of getting blocked by scraping politely, limiting load, and relying on cached results so the target sites are not hit too often.

### Step-by-step explanation
1. The scrapers send a realistic User-Agent instead of looking like a suspicious empty script.
2. Heavy crawlers use limited concurrency so they do not send too many requests at once.
3. Delays are added between requests where needed.
4. Retry with backoff is used in the heavier crawler flow to avoid repeated aggressive failures.
5. Most importantly, normal users never trigger live scraping. They read cached snapshots from MongoDB.
6. Only the cron routes perform the scraping on schedule, and those routes are protected with a secret.

### Design reasoning
If every user request triggered a live scrape, the app would be both slower and more likely to get blocked. Caching and polite scraping behavior make the whole system more stable.

### Possible improvement
I would build a shared retry-and-backoff wrapper and store the last scrape error in the database for faster debugging.

### Hard Terms Explained Simply
- `Rate limit` = when a site restricts how many requests you can make.
- `Backoff` = waiting longer before retrying after a failure.
- `Concurrency` = how many requests are being made at the same time.
