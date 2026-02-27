# ArmyVerse (Non‑Negotiable) — Authentication & Security

These answers are based on implemented code in `Documents/ARMYVERSE/` (not assumptions).

---

## 1) Explain your complete authentication flow.

### Short intro
ArmyVerse supports **two login styles**: Firebase login (Google/Twitter etc.) and custom username/password login using JWT. On every protected API route, I call one function `verifyAuth()` that can understand both token types.

### Step-by-step explanation
1. User signs in either:
   - Firebase Auth (client SDK), or
   - Username/password route (`/api/auth/signin`) which returns a JWT.
2. Frontend stores which auth type is active and attaches a bearer token on API requests.
3. On the backend, `verifyAuth(request)`:
   - tries to verify bearer token as Firebase ID token (if Firebase Admin is configured),
   - if that fails, it tries to verify it as JWT.
4. After the token is verified, backend normalizes user identity into a common object (`uid`, `email`, `authType`, etc.).
5. For JWT users, it also fetches user details from MongoDB.
6. API route continues only if auth is valid, otherwise returns `401`.

### Design reasoning (why I did it)
- I wanted to support both “easy social login” (Firebase) and “traditional login” (username/password) for users.
- A single `verifyAuth()` avoids inconsistent auth checks across routes.

### Possible improvement (1–2 lines)
I would add a single shared middleware wrapper for routes so auth + error handling is even more consistent.

### Hard Terms Explained Simply
- `Firebase ID token` = proof from Firebase that the user is logged in.
- `Bearer token` = token sent in the request header.

---

## 2) Why use Firebase + custom JWT together?

### Short intro
Firebase gives quick social login. JWT gives a simple username/password login path. Both are supported so the app can serve different user preferences.

### Step-by-step explanation
1. Firebase path is useful when user wants Google/Twitter login.
2. JWT path is useful when user wants a simple username-based account.
3. Both paths end in the same backend system because `verifyAuth()` normalizes them.

### Design reasoning (why I did it)
- This was a product + engineering choice: more flexibility with minimal extra code.
- Firebase Admin is only used if env vars are present, so local dev can still work.

### Possible improvement (1–2 lines)
I would clearly document “which routes require which auth” and add integration tests for both auth paths.

### Hard Terms Explained Simply
- `Normalize` = convert two different formats into one common format.
- `Admin SDK` = server-side library to verify Firebase tokens.

---

## 3) What is JWT and how are you using it?

### Short intro
JWT is a signed token that stores small identity info. I use it for username/password login so the backend can trust the token without calling Firebase.

### Step-by-step explanation
1. User signs in to `/api/auth/signin` with username/email + password.
2. Backend checks password (bcrypt) and creates a JWT signed with `JWT_SECRET`.
3. Frontend stores the JWT (in this app, it’s stored in `localStorage` as `auth_token`).
4. On every API call, frontend sends `Authorization: Bearer <jwt>`.
5. Backend verifies signature and expiry, then loads the user from MongoDB.

### Design reasoning (why I did it)
- JWT gives simple stateless sessions.
- It keeps the backend independent from Firebase for the username/password path.

### Possible improvement (1–2 lines)
Storing JWT in `localStorage` has XSS risk; for stronger security I would prefer HttpOnly cookies.

### Hard Terms Explained Simply
- `Stateless` = server doesn’t store session; token itself proves login.
- `JWT_SECRET` = secret key used to sign/verify JWT.

---

## 4) How are tokens stored?

### Short intro
For JWT auth, the token is stored in browser `localStorage`. For Firebase auth, Firebase SDK manages the session and provides an ID token when needed.

### Step-by-step explanation
1. JWT:
   - stored as `localStorage['auth_token']`
   - auth type stored as `localStorage['auth_type'] = 'jwt'`
2. Firebase:
   - Firebase SDK maintains login state
   - frontend can call `firebaseUser.getIdToken()` to get a token
3. API calls attach the active token as `Authorization: Bearer ...`.

### Design reasoning (why I did it)
- LocalStorage JWT is straightforward and easy to implement.
- Firebase SDK is standard for social login.

### Possible improvement (1–2 lines)
Move JWT storage to HttpOnly cookies and add stricter CSP to reduce token theft risk.

### Hard Terms Explained Simply
- `localStorage` = browser storage that survives refresh.
- `HttpOnly cookie` = cookie that JavaScript cannot read (safer).

---

## 5) What is PKCE and why did you use it?

### Short intro
PKCE is an OAuth safety method for cases where you don’t want to rely on a client secret. In my Spotify BYO flow, users may not provide a client secret, so PKCE is used.

### Step-by-step explanation
1. If user does not provide Spotify client secret, backend generates a `code_verifier`.
2. Backend hashes it into a `code_challenge`.
3. `code_challenge` goes to Spotify in the authorize URL.
4. I store the `code_verifier` temporarily (encrypted) in the user’s pending state.
5. On callback, token exchange includes the verifier so Spotify confirms it matches.

### Design reasoning (why I did it)
- BYO means users might have public-client style apps.
- PKCE lets OAuth work securely even without a stored client secret.

### Possible improvement (1–2 lines)
I would enforce strict expiry on pending PKCE state and log invalid-state attempts.

### Hard Terms Explained Simply
- `PKCE` = extra proof so attacker can’t reuse an OAuth code.
- `code_verifier/challenge` = secret and its hashed version.

---

## 6) What is AES-256-GCM and where did you use it?

### Short intro
AES-256-GCM is an encryption method that protects data and also detects tampering. I use it to encrypt Spotify BYO secrets before storing them in MongoDB.

### Step-by-step explanation
1. Server has `SPOTIFY_USER_SECRET_KEY`.
2. I derive a 32-byte key (AES-256) using SHA-256.
3. For each secret, I generate a random IV (nonce) and encrypt.
4. I store `iv.ciphertext.tag` as a single string.
5. On read, I decrypt using the same key and verify tag.

### Design reasoning (why I did it)
- BYO refresh tokens and client secrets are sensitive.
- If DB is leaked, encrypted values reduce damage.

### Possible improvement (1–2 lines)
Add key rotation support (versioned encryption) so secrets can be rotated safely.

### Hard Terms Explained Simply
- `Encryption at rest` = encrypt before saving to database.
- `Auth tag` = proof that encrypted data was not modified.

---

## 7) How do you prevent XSS and CSRF?

### Short intro
I prevent some security issues through auth design and OAuth state checks, but I do **not** claim full XSS/CSRF hardening everywhere because the repo shows some places that still need improvement.

### Step-by-step explanation
1. CSRF:
   - Most APIs use `Authorization: Bearer ...` tokens (not cookie sessions), which reduces classic CSRF risk.
   - Spotify OAuth callback uses a `state` parameter check to prevent login swapping.
2. XSS:
   - React by default escapes strings.
   - However, blog content is stored as HTML and rendered using `dangerouslySetInnerHTML`, and I do not see server-side HTML sanitization on blog save. That is a real XSS risk if untrusted HTML is saved.

### Design reasoning (why I did it)
- For OAuth, state protection is non-negotiable, so it is implemented.
- For blogs/editor, the focus was feature completeness first; sanitization should be added before calling it production-grade.

### Possible improvement (1–2 lines)
Sanitize blog HTML on save (and/or on render) and add a Content Security Policy (CSP).

### Hard Terms Explained Simply
- `XSS` = attacker injects script into a page.
- `CSRF` = attacker tricks your browser into sending a request you didn’t intend.

---

## 8) What are possible security vulnerabilities in ArmyVerse?

### Short intro
The biggest real risks in this repo are HTML XSS surfaces, token storage in localStorage, and missing RBAC enforcement even though a role field exists.

### Step-by-step explanation
1. Blog XSS risk: storing/rendering raw HTML without strong sanitization.
2. JWT in localStorage: if XSS happens, tokens can be stolen.
3. Missing RBAC enforcement: `User.role` exists but I do not see route-level checks.
4. External API abuse: endpoints that call Spotify/Last.fm can be abused without strong global rate limiting.

### Design reasoning (why I did it)
- As a fresher project, I built working systems first and documented next hardening steps.
- Some controls (OAuth state, encryption-at-rest) are implemented because they are high impact.

### Possible improvement (1–2 lines)
Add systematic input/output sanitization, RBAC checks, and rate limiting on expensive endpoints.

### Hard Terms Explained Simply
- `RBAC` = role-based access control.
- `Attack surface` = all places where an attacker can try something.

---

## 9) How does role-based access work?

### Short intro
In the current code, there is a `role` field in the `User` model (`user` or `admin`). But I do not see route checks that enforce admin-only actions.

### Step-by-step explanation
1. User schema has `role: 'user' | 'admin'`.
2. API routes generally verify authentication.
3. I do not see `if (user.role !== 'admin')` checks in routes.

### Design reasoning (why I did it)
- I added the role field early to keep the schema ready.
- But role enforcement would need explicit checks for admin endpoints.

### Possible improvement (1–2 lines)
Implement RBAC helpers and enforce them in any admin-sensitive route (scrapers, moderation, deletion).

### Hard Terms Explained Simply
- `Role` = label like admin/user.
- `Enforce` = actually block actions when role is wrong.

---

## 10) How do you secure API routes?

### Short intro
Protected routes call `verifyAuth()` and return `401` if user is not valid. Cron routes require a secret header.

### Step-by-step explanation
1. For user routes (quiz, inventory, claim, spotify status):
   - read bearer token
   - `verifyAuth()`
   - continue only if user exists
2. For cron routes:
   - require `Authorization: Bearer <CRON_SECRET>` (or Vercel cron header)
   - allow local bypass only when explicitly enabled in non-production
3. For Spotify BYO secrets:
   - encrypt stored secrets
   - validate state during callback

### Design reasoning (why I did it)
- Auth-first is simplest and most reliable for a large set of endpoints.
- Cron secret prevents random internet callers from triggering heavy jobs.

### Possible improvement (1–2 lines)
Add centralized middleware and rate limits, plus detailed audit logs for sensitive actions.

### Hard Terms Explained Simply
- `401 Unauthorized` = you are not logged in or token is invalid.
- `Cron secret` = secret string needed to trigger scheduled jobs.
