# ArmyVerse (Non‑Negotiable) — Gamification System (Boraverse)

This is based on implemented code in:
- `app/api/game/*`
- `lib/game/*`
- `lib/models/*`

---

## 1) Explain your RNG system.

### Short intro
RNG is mainly used for photocard drops. In the current implementation, the rarity is effectively `random`, and the card is selected using MongoDB sampling.

### Step-by-step explanation
1. User completes an action that can award a card (quiz, quest ticket, BoraRush completion).
2. Backend decides whether a card drop is allowed (example: quiz requires XP >= 5).
3. To pick a card, backend uses MongoDB aggregation with `$sample: { size: 1 }` on the photocard catalog collection.
4. The selected card is mapped into a response-friendly shape.
5. The grant is written to inventory (or converted to dust if already owned).
6. An audit record is created for traceability.

### Design reasoning (why I did it)
- `$sample` is the simplest way to do random selection from a large catalog.
- Keeping “rarity” as `random` avoids complicated balancing logic until it is truly needed.

### Possible improvement (1–2 lines)
If I wanted balancing, I would introduce weighted pools and guaranteed-progression logic instead of pure uniform sampling.

### Hard Terms Explained Simply
- `RNG` = randomness used to pick rewards.
- `$sample` = MongoDB feature to pick random documents.

---

## 2) How does pity system work?

### Short intro
In the current code, the pity/rarity system is **not active**. The drop code returns `rarity: 'random'` and `pityApplied: false`.

### Step-by-step explanation
1. Quiz completion computes XP.
2. If XP < 5, no card drop.
3. If XP >= 5, it picks a random photocard.
4. The returned response includes fields like `pityApplied`, but the implementation currently sets it to `false`.

### Design reasoning (why I did it)
- I kept the system simple and stable first.
- The code structure still allows a future pity system without rewriting everything.

### Possible improvement (1–2 lines)
If users complain about unfair randomness, I would add a real pity counter in `UserGameState` and enforce it in the drop function.

### Hard Terms Explained Simply
- `Pity system` = rule that reduces “bad luck streaks”.
- `Balancing` = keeping rewards fair over time.

---

## 3) How did you prevent cheating?

### Short intro
For the external BoraRush game, the main anti-cheat controls are: short-lived handoff tokens, allowed origins (CORS), idempotent run IDs, and daily caps.

### Step-by-step explanation
1. ArmyVerse issues a BoraRush handoff token from `/api/game/borarush/handoff` after verifying the user.
2. BoraRush uses that token to submit results to `/api/game/borarush/complete`.
3. Backend verifies:
   - token signature, audience, issuer
   - run schema (`turns` clamped, `runId` required)
   - run idempotency: if `runId` already exists, it returns duplicate response
4. Backend enforces daily caps using `BoraRushDailyLimit` counters.
5. Backend awards XP and card only if caps allow.

### Design reasoning (why I did it)
- The external game is the highest cheating risk because the client could fake scores.
- So the server must be the decision-maker for awards.

### Possible improvement (1–2 lines)
Add stronger anomaly detection (impossible turn patterns, repeated suspicious run submissions) and better audit dashboards.

### Hard Terms Explained Simply
- `Idempotency` = same run submitted twice should not give reward twice.
- `Daily cap` = maximum rewards per day.

---

## 4) How do you handle concurrency?

### Short intro
Concurrency issues happen when the same user triggers multiple requests at the same time (double clicks, retries). I handle it mainly using idempotency checks and atomic updates.

### Step-by-step explanation
1. Quiz session can be completed only once because session status changes from `active` to `completed`.
2. Quest claim checks `prog.claimed` and returns `409` if already claimed.
3. BoraRush completion checks `runId` existence to avoid duplicate rewards.
4. Daily caps use a “reserve then award” approach with atomic `$inc` on counters.
5. If an error happens after reserving caps, the code tries to roll back counters.

### Design reasoning (why I did it)
- In web apps, retries and double submits are normal.
- Idempotent design makes the system safe without needing heavy locking.

### Possible improvement (1–2 lines)
For high-value reward flows, I would use MongoDB transactions for grouped writes to prevent partial updates.

### Hard Terms Explained Simply
- `Concurrency` = multiple requests at the same time.
- `Atomic update` = DB change that happens as one safe operation.

---

## 5) What happens if two pulls happen at same time?

### Short intro
If “two pulls” means two reward-grant requests in parallel, the goal is: user should not get double rewards accidentally.

### Step-by-step explanation
1. For quest claims, the `claimed` flag blocks the second claim.
2. For BoraRush, the same `runId` blocks duplicates.
3. For quiz completion, the session status prevents re-complete of the same session.
4. For inventory grants, the code checks ownership and duplicates convert to dust instead of adding same card again.

### Design reasoning (why I did it)
- Users double click or network retries happen.
- The backend must be safe even if the frontend sends twice.

### Possible improvement (1–2 lines)
Add unique DB constraints (business keys) for grants so DB enforces “only once” too.

### Hard Terms Explained Simply
- `Duplicate request` = the same action sent twice.
- `Business key` = unique identifier like `(userId, questCode, periodKey)`.

---

## 6) How did you design leaderboard?

### Short intro
Leaderboard is designed by time periods: daily, weekly, and all-time. Each period has a `periodKey` and stores score and stats.

### Step-by-step explanation
1. When a user earns XP in ranked mode, leaderboard entries are updated for multiple periods.
2. Each entry stores:
   - `periodKey` (daily-YYYY-MM-DD, weekly-YYYY-WW, or alltime)
   - `score` (XP)
   - `level` (derived from total XP)
   - stats like quizzes played and accuracy
3. When client requests leaderboard, backend:
   - queries entries for that period
   - sorts by score
   - lazily updates ranks (bulk write)
4. There is also an all-time backfill behavior if the user has XP but missing all-time entry.

### Design reasoning (why I did it)
- Period-based keys make resets easy without deleting everything.
- It also makes it easy to compare “this week” vs “all time”.

### Possible improvement (1–2 lines)
Move rank recomputation to a scheduled job so leaderboard reads stay fast at scale.

### Hard Terms Explained Simply
- `Period key` = string that identifies the leaderboard time window.
- `Bulk write` = updating many DB rows in one operation.

---

## 7) What is ISO week and why used?

### Short intro
ISO week means the week starts on Monday and week numbers are consistent. I use ISO-style week logic to build weekly leaderboard keys.

### Step-by-step explanation
1. For weekly leaderboards, I compute the Monday start date in UTC.
2. I compute week number based on that start date.
3. I create `periodKey` like `weekly-2026-02`.
4. This gives consistent weekly boundaries.

### Design reasoning (why I did it)
- Weekly boundaries should not depend on local time.
- ISO-style weeks are common in reporting and competitions.

### Possible improvement (1–2 lines)
Use a battle-tested date library for week numbers to avoid edge-case mistakes around year boundaries.

### Hard Terms Explained Simply
- `ISO week` = standard way to define weeks (Monday start).
- `UTC` = global time standard, avoids timezone confusion.
