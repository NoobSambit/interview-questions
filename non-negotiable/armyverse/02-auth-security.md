# ArmyVerse (Non-Negotiable) - Authentication & Security

These answers are based on the implemented ArmyVerse project, but rewritten in simple interview language so they are easier to remember and speak clearly.

---

## 1) Explain your complete authentication flow.

### Short intro
ArmyVerse supports two login styles: Firebase-based login and custom username-password login with JWT. On protected routes, the backend uses one shared verification flow so both login styles can be handled in a consistent way.

### Step-by-step explanation
1. The user signs in either through Firebase or through the custom signin route.
2. The frontend stores which auth type is active and sends a bearer token with protected API requests.
3. On the backend, `verifyAuth()` checks the incoming token.
4. It first tries to understand the token as a Firebase token if Firebase is configured.
5. If that does not work, it tries to verify the token as a JWT.
6. Once the token is verified, the backend converts the result into one common user identity shape.
7. For JWT users, it also loads the user from MongoDB.
8. If verification fails, the route returns `401` and the action stops.

### Design reasoning
I wanted login flexibility for users while keeping the backend logic simple. A shared verification function avoids repeating different auth logic in many routes.

### Possible improvement
I would wrap route auth and common error handling into a stronger shared middleware-style helper.

### Hard Terms Explained Simply
- `Firebase token` = proof from Firebase that the user is logged in.
- `Bearer token` = the token sent in the request header.
- `JWT` = a signed token that stores small identity information.

---

## 2) Why use Firebase + custom JWT together?

### Short intro
I used both because they solve different product needs. Firebase makes social login easy, while JWT supports a more traditional username-password flow.

### Step-by-step explanation
1. Some users prefer quick social login like Google or Twitter.
2. Some users prefer creating a normal username-based account.
3. Firebase handles the social login side well.
4. JWT handles the custom login side without depending on Firebase for everything.
5. On the backend, both are normalized into the same identity shape, so the rest of the app does not need to care much about which login path was used.

### Design reasoning
This gave product flexibility without making the rest of the backend too complicated.

### Possible improvement
I would document both auth paths more clearly and add more tests to verify both flows end-to-end.

### Hard Terms Explained Simply
- `Normalize` = convert different inputs into one common format.
- `Admin SDK` = the backend library used to verify Firebase tokens securely.

---

## 3) What is JWT and how are you using it?

### Short intro
JWT is a signed token that the backend can verify later. In ArmyVerse, I use it for the username-password login path.

### Step-by-step explanation
1. The user signs in using username or email plus password.
2. The backend checks the password with bcrypt.
3. If the password is correct, the backend creates a JWT signed with a secret key.
4. The frontend stores that token and sends it in future API calls.
5. The backend verifies the token signature and expiry on each protected request.
6. After that, it loads the real user record from MongoDB and continues the request.

### Design reasoning
JWT gave me a simple stateless login flow for the custom auth path.

### Possible improvement
For stronger browser security, I would prefer HttpOnly cookies over localStorage for JWT storage.

### Hard Terms Explained Simply
- `Stateless` = the server does not keep session data in memory between requests.
- `Signature` = proof that the token was created by the backend and not modified by someone else.
- `Expiry` = the time after which the token is no longer valid.

---

## 4) How are tokens stored?

### Short intro
JWT tokens are stored in browser localStorage, while Firebase login state is managed by the Firebase SDK and the frontend asks it for an ID token when needed.

### Step-by-step explanation
1. If the user logs in with the custom auth system, the frontend stores the JWT in localStorage.
2. It also stores which auth type is active.
3. If the user logs in with Firebase, the Firebase SDK manages the login session.
4. When the frontend needs to call a protected API, it gets the correct token for the active auth type.
5. That token is then sent in the `Authorization` header.

### Design reasoning
This was the simplest working setup for supporting both login paths in one frontend.

### Possible improvement
I would move JWT storage to HttpOnly cookies and add stronger browser security rules like CSP.

### Hard Terms Explained Simply
- `localStorage` = browser storage that survives refreshes.
- `HttpOnly cookie` = a browser cookie that JavaScript cannot read directly, which is safer.
- `CSP` = a browser security policy that restricts unsafe scripts and resources.

---

## 5) What is PKCE and why did you use it?

### Short intro
PKCE is an OAuth security mechanism. I used it in the Spotify BYO flow because some users may connect with their own Spotify app setup without relying on a client secret.

### Step-by-step explanation
1. In the BYO flow, the backend can generate a secret temporary value called a verifier.
2. It hashes that verifier into a challenge.
3. The challenge is sent to Spotify during the authorization step.
4. The verifier is stored temporarily on the backend side.
5. During the callback, the backend sends the verifier back to Spotify.
6. Spotify checks that the verifier matches the earlier challenge.
7. If it matches, Spotify knows the same flow is completing and not some attacker.

### Design reasoning
PKCE was important because BYO needs a secure OAuth flow even when a user is not depending on one fully centralized secret setup.

### Possible improvement
I would make expiry and cleanup of pending PKCE state even stricter and log invalid-state attempts more clearly.

### Hard Terms Explained Simply
- `PKCE` = extra proof used in OAuth so an attacker cannot easily hijack the login flow.
- `Verifier` = the secret temporary value.
- `Challenge` = the hashed version of that verifier.

---

## 6) What is AES-256-GCM and where did you use it?

### Short intro
AES-256-GCM is a strong encryption method. I used it to encrypt sensitive Spotify BYO values before saving them in MongoDB.

### Step-by-step explanation
1. The server keeps an encryption secret in environment variables.
2. That secret is used to derive the encryption key.
3. Before saving sensitive values like refresh tokens or client secrets, the backend encrypts them.
4. The encrypted form is stored in MongoDB instead of raw secret text.
5. When the backend needs to use those values later, it decrypts them on the server side.
6. The client never receives the raw stored secrets.

### Design reasoning
If the database is leaked, encrypted secrets are far safer than plain text secrets. Since BYO tokens are sensitive, encrypting them was necessary.

### Possible improvement
I would add key rotation support so secrets can be re-encrypted safely over time.

### Hard Terms Explained Simply
- `Encryption at rest` = encrypting data before saving it in the database.
- `Auth tag` = extra proof used to detect whether encrypted data was modified.
- `Key rotation` = changing encryption keys safely over time.

---

## 7) How do you prevent XSS and CSRF?

### Short intro
I reduce some common security risks, but I would not claim the project is fully hardened everywhere. The codebase shows some protections already, and also some areas that still need improvement.

### Step-by-step explanation
1. For CSRF, the system mostly uses bearer tokens instead of cookie-based sessions, which reduces the classic CSRF risk.
2. In OAuth flows, I use the `state` parameter so callback requests are tied to the correct user flow.
3. For XSS, React helps by escaping normal text by default.
4. But blog content can be stored and rendered as HTML, and that creates a real XSS risk if the HTML is not properly sanitized.
5. So the honest answer is: some protections are present, but blog HTML is still an important hardening area.

### Design reasoning
I implemented the most critical protections first, especially around auth and OAuth correctness. For content rendering, the feature came first and stronger sanitization should be added before calling it production-grade.

### Possible improvement
I would sanitize stored HTML, sanitize again on render if needed, and add a strong Content Security Policy.

### Hard Terms Explained Simply
- `XSS` = when unsafe script gets injected into a page.
- `CSRF` = when a browser is tricked into sending an unwanted request.
- `Sanitize` = clean input so unsafe code or content is removed.

---

## 8) What are possible security vulnerabilities in ArmyVerse?

### Short intro
The biggest realistic risks are HTML-based XSS surfaces, token theft risk from localStorage, and incomplete authorization enforcement even though role fields exist.

### Step-by-step explanation
1. If untrusted HTML is stored and rendered, that can become an XSS issue.
2. If JWT tokens live in localStorage and XSS happens, those tokens can be stolen.
3. The user model has roles, but I do not see strong route-level RBAC enforcement everywhere.
4. Some external-integration endpoints could also be abused if global rate limiting is weak.
5. So the biggest risks are not fancy crypto failures. They are mostly application-level hardening gaps.

### Design reasoning
As a fresher project, I focused first on getting working flows live and then identifying the next hardening steps clearly.

### Possible improvement
I would add sanitization, RBAC enforcement, rate limiting, and better audit logs on sensitive endpoints.

### Hard Terms Explained Simply
- `RBAC` = role-based access control, meaning actions are limited by user role.
- `Attack surface` = all the places where an attacker might try to exploit the system.
- `Audit log` = a record of sensitive actions for debugging and security review.

---

## 9) How does role-based access work?

### Short intro
The schema is prepared for role-based access, because users have a role field like `user` or `admin`. But in the current implementation, I would say role enforcement is incomplete.

### Step-by-step explanation
1. The user model stores a role value.
2. Most routes do check whether the user is authenticated.
3. But authentication alone is not the same as authorization.
4. I do not see strong route checks everywhere that say only admins can do certain sensitive actions.
5. So the role system exists at the data level, but it still needs stronger enforcement at the route level.

### Design reasoning
I added the role field early so the schema would be ready. The next step would be consistently applying that role data in the actual API protections.

### Possible improvement
I would create shared RBAC helpers and apply them to admin-only routes such as moderation, deletion, or sensitive cron-like actions.

### Hard Terms Explained Simply
- `Authorization` = deciding what a logged-in user is allowed to do.
- `Enforcement` = actually blocking the action if the user does not have permission.

---

## 10) How do you secure API routes?

### Short intro
The main protection is that sensitive routes verify the user first. Cron routes also use a secret so random public callers cannot trigger them.

### Step-by-step explanation
1. Protected user routes read the bearer token and call `verifyAuth()`.
2. If the token is invalid, the request stops with `401`.
3. If the token is valid, the route continues with the user identity attached.
4. For cron routes, the request must include a known secret token, unless a special local development bypass is intentionally enabled.
5. For Spotify BYO flows, stored secrets are encrypted and OAuth callback safety checks are applied.
6. So route protection combines authentication, secret-protected internal routes, and safer handling of integration credentials.

### Design reasoning
With many routes in the app, the safest default is auth-first route design. That keeps the system consistent and easier to audit.

### Possible improvement
I would add centralized middleware-like wrappers, rate limits, and stronger auditing for sensitive actions.

### Hard Terms Explained Simply
- `401 Unauthorized` = the request is not allowed because login is missing or invalid.
- `Audit` = reviewing who did what and when.
- `Rate limit` = restricting how often a client can call an endpoint.
