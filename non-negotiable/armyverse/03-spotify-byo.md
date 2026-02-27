# ArmyVerse (Non‑Negotiable) — Spotify BYO App System

Everything below is based on these implemented areas:
- `app/api/spotify/client-credentials/route.ts`
- `app/api/spotify/callback/route.ts`
- `app/api/spotify/status/route.ts`
- `lib/utils/secrets.ts`
- `lib/spotify/userTokens.ts`
- `app/api/playlist/export/route.ts`

---

## 1) Explain Spotify OAuth flow step-by-step.

### Short intro
I support two Spotify OAuth flows:
1. **Standard**: user connects using ArmyVerse’s centralized Spotify app.
2. **BYO**: user connects using their own Spotify app credentials.

Both flows end by storing Spotify integration info inside the user document, then the app can call Spotify APIs on the user’s behalf.

### Step-by-step explanation
1. User starts connect from UI.
2. Backend builds a Spotify authorize URL.
3. User approves on Spotify and Spotify redirects to `/api/spotify/callback` with `code` and `state`.
4. Callback exchanges `code` for tokens via Spotify `/api/token`.
5. Callback calls Spotify `/v1/me` to confirm which Spotify account is connected.
6. Backend stores tokens + profile metadata in MongoDB under `User.integrations.spotify` (standard) or `User.integrations.spotifyByo` (BYO).
7. UI uses `/api/spotify/status` to show connected state and to fetch a usable access token.

### Design reasoning (why I did it)
- This is the standard OAuth pattern and is interview-friendly to explain.
- Calling `/me` right after token exchange reduces confusion about which account is connected.

### Possible improvement (1–2 lines)
Add stricter expiry checks for pending states and add more structured logs for callback failures.

### Hard Terms Explained Simply
- `OAuth` = “login with Spotify” flow that gives the app permission.
- `Access token` = short-lived token used for API calls.

---

## 2) What problem were you solving with BYO app?

### Short intro
BYO is for users who want exports to always go to **their** Spotify account using **their** Spotify app credentials. It also reduces dependency on one centralized app quota/config.

### Step-by-step explanation
1. Standard flow uses one Spotify app for everyone.
2. Some users want more control (their own client id/secret) or avoid issues when the centralized app is rate-limited or in development mode restrictions.
3. BYO lets them connect their own Spotify app and store refresh tokens encrypted.
4. Then export uses their tokens and creates playlists inside their own Spotify account.

### Design reasoning (why I did it)
- It makes “export to my account” more reliable.
- It also makes the system more flexible for different environments.

### Possible improvement (1–2 lines)
Add UI warnings and validation so users understand Spotify dev mode restrictions and scopes clearly.

### Hard Terms Explained Simply
- `Quota/rate limit` = how many API calls Spotify allows in a time window.
- `Client ID/Secret` = Spotify app credentials.

---

## 3) How did you encrypt Client ID and Secret?

### Short intro
For BYO, I encrypt sensitive values before saving them in MongoDB using AES-256-GCM.

### Step-by-step explanation
1. Server has an env secret: `SPOTIFY_USER_SECRET_KEY`.
2. `lib/utils/secrets.ts` derives a 32-byte key from it.
3. Before storing BYO values, backend encrypts:
   - `clientId`
   - `clientSecret` (optional)
   - `refreshToken`
   - `code_verifier` (temporary PKCE value)
4. Encrypted value is saved as a string in `User.integrations.spotifyByo.*Enc`.
5. When needed (token refresh), backend decrypts, uses it, and never sends raw secrets to the client.

### Design reasoning (why I did it)
- Database leaks are realistic; encryption-at-rest reduces damage.
- AES-256-GCM also detects tampering.

### Possible improvement (1–2 lines)
Add key rotation with versioning so old encrypted values can be migrated safely.

### Hard Terms Explained Simply
- `AES-256-GCM` = strong encryption that also detects changes.
- `Encryption at rest` = encrypting before saving to DB.

---

## 4) What is rate limiting and how did you implement it?

### Short intro
Rate limiting means controlling how fast we call an API so we don’t get blocked. In ArmyVerse, I implemented a token-bucket style limiter inside the Last.fm client.

### Step-by-step explanation
1. Last.fm client has a small “request budget” (tokens).
2. Each API call consumes 1 token.
3. Tokens refill gradually (configured as about 5 tokens per second).
4. If tokens are empty, code waits a short time and then continues.
5. This smooths bursts and avoids spamming the provider.

### Design reasoning (why I did it)
- Quest verification can call Last.fm many times.
- Without rate limiting, you can hit provider limits and make UX unreliable.

### Possible improvement (1–2 lines)
This limiter is in-memory per server instance; at scale I would use a shared store (like Redis) for distributed rate limiting.

### Hard Terms Explained Simply
- `Rate limiting` = controlling request speed.
- `Token bucket` = a small allowance that refills over time.

---

## 5) What happens when rate limit exceeds?

### Short intro
In the current limiter design, we try not to exceed the limit by waiting. So the most common effect is **slower response**, not immediate failure.

### Step-by-step explanation
1. When there are no tokens left, `acquire()` waits for enough refill.
2. After waiting, it continues and does the request.
3. If provider still rejects (network error or provider-side limit), the call throws and the route returns an error.

### Design reasoning (why I did it)
- Waiting is better than hammering the API and getting blocked.

### Possible improvement (1–2 lines)
Add clear error codes to distinguish “provider down” vs “we are throttling”.

### Hard Terms Explained Simply
- `Throttle` = intentionally slow down.
- `Provider` = external API like Last.fm.

---

## 6) How does token refresh work?

### Short intro
Token refresh means using a long-lived refresh token to get a new short-lived access token. I do this for both standard Spotify integration and BYO.

### Step-by-step explanation
1. `/api/spotify/status` checks if access token is near expiry.
2. If yes, it calls Spotify token endpoint with `grant_type=refresh_token`.
3. For standard flow, it uses centralized client id/secret.
4. For BYO, it decrypts the user’s BYO client credentials and refresh token and uses those.
5. It updates stored access token + expiry, and returns a valid access token to the client.

### Design reasoning (why I did it)
- Users should not need to reconnect every time access token expires.
- BYO refresh also supports Spotify refresh-token rotation when Spotify returns a new refresh token.

### Possible improvement (1–2 lines)
Add more explicit “token health” telemetry and alerts for revoke/invalid_client patterns.

### Hard Terms Explained Simply
- `Refresh token` = long-lived token used to get a new access token.
- `ExpiresAt` = time when access token becomes invalid.

---

## 7) What is state parameter in OAuth?

### Short intro
`state` is a safety value that binds the OAuth callback to the correct user/session. It prevents attackers from swapping the login response.

### Step-by-step explanation
1. When starting OAuth, backend generates a random state.
2. In BYO flow, that state is stored in the user document under `pending.spotifyByo.state`.
3. Spotify returns the same state back to callback.
4. Callback finds the user with matching pending state.
5. If state is missing or mismatched, callback rejects.
6. In standard flow, state is HMAC-signed and timestamped, then verified on callback.

### Design reasoning (why I did it)
- Without state validation, OAuth callback can be abused (CSRF / session mix-up).

### Possible improvement (1–2 lines)
Add strict TTL checks for pending BYO state and cleanup of stale pending records.

### Hard Terms Explained Simply
- `state` = random safety string stored before redirect.
- `HMAC` = a signature used to prove data wasn’t changed.
