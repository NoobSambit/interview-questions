# ArmyVerse (Non-Negotiable) - Gamification System (Boraverse)

These answers are based on the implemented ArmyVerse project, but rewritten in simple interview language so they are easier to remember and explain.

---

## 1) Explain your RNG system.

### Short intro
In ArmyVerse, RNG is mainly used for reward drops like photocards. The current version keeps the randomness simple instead of trying to over-engineer balancing too early.

### Step-by-step explanation
1. The user completes an action that can potentially award a reward, such as a quiz or BoraRush completion.
2. The backend first checks whether the user is eligible for a drop.
3. If a drop is allowed, the system selects a photocard randomly from the catalog.
4. That random selection is done using MongoDB sampling.
5. After selection, the reward is written to inventory.
6. If the user already owns the card, the system can convert that duplicate into dust instead of giving the exact same card again.
7. An audit record is also created so the action can be traced later.

### Design reasoning
I kept RNG simple because that made the system easier to build and explain. For an early-stage project, plain random selection is often better than adding complex balancing too soon.

### Possible improvement
If balancing becomes important, I would add weighted rarity pools and progression-based guarantees.

### Hard Terms Explained Simply
- `RNG` = randomness used to choose rewards.
- `Sampling` = selecting a random record from the database.
- `Audit record` = a saved log showing that a reward action happened.

---

## 2) How does pity system work?

### Short intro
In the current code, the pity system is not truly active yet. The structure is there, but the actual reward logic still behaves like a simple random system.

### Step-by-step explanation
1. The quiz flow first computes whether the user earned enough to qualify for a drop.
2. If the user does not qualify, there is no card reward.
3. If the user qualifies, the system picks a random photocard.
4. The response still contains fields related to pity behavior, but they are not actually driving reward logic right now.
5. So the honest answer is that pity is prepared in the response shape, but not fully implemented as a real fairness system yet.

### Design reasoning
I chose to keep the reward system stable and understandable first. That is often better than shipping a half-balanced pity system too early.

### Possible improvement
I would store a pity counter in the user game state and use it to improve fairness after repeated bad luck streaks.

### Hard Terms Explained Simply
- `Pity system` = a rule that protects users from getting unlucky for too long.
- `Fairness system` = logic that tries to make rewards feel less frustrating over time.

---

## 3) How did you prevent cheating?

### Short intro
The external BoraRush game had the highest cheating risk, so the backend was designed to be the final authority for rewards. The client can report results, but the server decides whether those results are accepted.

### Step-by-step explanation
1. ArmyVerse first issues a short-lived handoff token after verifying the user.
2. BoraRush uses that token when submitting completion data back to the backend.
3. The backend verifies the token details and the incoming run data.
4. It requires a run ID and checks if that run ID was already used before.
5. If the same run is submitted again, the system treats it as a duplicate instead of giving rewards twice.
6. It also enforces daily reward limits.
7. Only after all those checks pass does it award XP or card-related rewards.

### Design reasoning
Any system where an external client reports achievements can be abused. So the backend must verify identity, validate the run, and prevent duplicate reward claims.

### Possible improvement
I would add anomaly detection for suspicious run patterns and stronger dashboards for reviewing abuse.

### Hard Terms Explained Simply
- `Idempotency` = the same action should not give rewards twice if submitted again.
- `Daily cap` = the maximum number of rewards allowed in one day.
- `Anomaly detection` = finding unusual behavior that may suggest cheating.

---

## 4) How do you handle concurrency?

### Short intro
Concurrency problems happen when the same user triggers multiple requests at nearly the same time. I handled that mainly through idempotency checks, state checks, and safe database updates.

### Step-by-step explanation
1. Quiz sessions can only be completed once because their status changes after completion.
2. Quest claims check whether the reward was already claimed.
3. BoraRush submissions check whether the same run ID already exists.
4. Daily caps are updated carefully using database increments.
5. If something fails after a partial reserve, the code attempts to roll back the temporary reservation.
6. So the system tries to stay safe even when users double click, retry, or network issues cause repeated submissions.

### Design reasoning
In real apps, duplicate requests are normal. Safe concurrency handling keeps users from accidentally getting double rewards or corrupting progress.

### Possible improvement
For the most valuable reward paths, I would use MongoDB transactions to make grouped writes even safer.

### Hard Terms Explained Simply
- `Concurrency` = multiple requests happening at nearly the same time.
- `Atomic update` = a database change that happens safely as one operation.
- `Transaction` = a way to group multiple database changes so they succeed or fail together.

---

## 5) What happens if two pulls happen at same time?

### Short intro
If two reward requests happen in parallel, the goal is simple: the user should not accidentally get rewarded twice for the same action.

### Step-by-step explanation
1. In quest claims, the claimed flag blocks a second reward attempt.
2. In BoraRush, the run ID blocks duplicate submissions of the same run.
3. In quiz completion, the session status blocks completing the same session twice.
4. In inventory logic, duplicates can be converted into dust rather than creating invalid repeated ownership.
5. So the system protects against both user double-clicks and automatic network retries.

### Design reasoning
The backend should be safe even if the frontend behaves imperfectly or the same request arrives twice.

### Possible improvement
I would add stronger unique constraints in the database so correctness is enforced both in code and at the DB level.

### Hard Terms Explained Simply
- `Duplicate request` = the same action being sent more than once.
- `Unique constraint` = a database rule saying a certain combination can only exist once.

---

## 6) How did you design leaderboard?

### Short intro
The leaderboard is designed around time windows like daily, weekly, and all-time. That makes it easier to support fresh competition while also keeping long-term progress.

### Step-by-step explanation
1. When a user earns ranked XP, the backend updates leaderboard entries for the relevant time periods.
2. Each leaderboard entry stores a period key, score, level, and supporting stats.
3. Daily and weekly leaderboards show recent competition.
4. All-time leaderboard shows long-term progress.
5. When the client requests a leaderboard, the backend reads entries for that period, sorts them, and updates rank information.
6. There is also support for backfilling all-time entries if a user has XP but is missing an expected record.

### Design reasoning
Period-based design makes resets simple and keeps the leaderboard system flexible. It is much easier to compare today, this week, and all time when the data is already separated that way.

### Possible improvement
I would move rank recomputation to a scheduled job so leaderboard reads stay lighter at scale.

### Hard Terms Explained Simply
- `Period key` = the label that identifies a leaderboard time window.
- `Backfill` = creating missing historical or expected records after the fact.
- `Rank recomputation` = recalculating who is 1st, 2nd, 3rd, and so on.

---

## 7) What is ISO week and why used?

### Short intro
ISO week is a standard way of defining weeks, usually with Monday as the first day. I used it so weekly leaderboards stay consistent and do not depend on local timezone confusion.

### Step-by-step explanation
1. For weekly leaderboards, I compute the week using a standard week boundary.
2. I base it on UTC instead of local device time.
3. That creates a consistent weekly period key.
4. As a result, all users are compared under the same weekly window instead of different local interpretations.

### Design reasoning
A competition system needs stable time boundaries. Using ISO-style week logic makes weekly reporting more predictable and fair.

### Possible improvement
I would rely on a stronger date utility library for tricky year-boundary edge cases.

### Hard Terms Explained Simply
- `ISO week` = a standard way to count weeks, usually starting on Monday.
- `UTC` = a global standard time reference used to avoid timezone confusion.
- `Edge case` = an unusual situation that can break normal logic if not handled carefully.
