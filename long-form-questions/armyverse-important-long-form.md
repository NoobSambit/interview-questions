# ArmyVerse - Important Long-Form Interview Questions

These are explain-style questions for 3-8 minute answers.
Focus on architecture decisions, tradeoffs, debugging stories, and security reasoning.

## A) Project Story and Architecture

1. Give a complete 5-minute architecture walkthrough of ArmyVerse from user request to database write and UI response.
2. Explain why you chose this architecture as a final-year fresher project instead of a heavier microservice setup.
3. If you had to explain ArmyVerse to a non-technical interviewer, how would you describe the core value and technical depth?
4. Walk through the full lifecycle of a user action: start quest -> verify completion -> claim reward -> leaderboard update.
5. What were the top 3 technical risks while building ArmyVerse, and how did you reduce each one?
6. Which module in ArmyVerse is most production-ready today, and why?
7. Which module is least production-ready today, and what exact hardening work is pending?
8. If DAU grows from 1k to 100k, what exact scaling plan would you execute in phases?

## B) Authentication, Authorization, and Identity

9. Explain the full authentication model in ArmyVerse and how you protect user identity across routes.
10. Walk through how JWT/session/user validation works and where mistakes can cause unauthorized access.
11. Explain Spotify OAuth callback flow end-to-end and how you defend against state mismatch attacks.
12. Compare Firebase-auth style identity checks vs app-issued tokens in your architecture.
13. How do you guarantee that a user can only claim rewards for their own account and never another user?
14. If an interviewer says "Your auth looks okay but not enterprise-grade," how do you respond with concrete improvements?

## C) Security and Secrets

15. Explain your encryption design (AES-256-GCM style flow) and why each component exists.
16. What happens if database data is leaked but environment secrets are safe?
17. What happens if both database data and environment secrets are leaked?
18. Describe your secret management strategy for local, staging, and production.
19. How would you implement safe key rotation without breaking existing encrypted records?
20. Which 5 security improvements would you implement first if this goes to real production?

## D) Game System and Economy

21. Explain your XP, rewards, and progression logic so that both product and engineering interviewers understand it.
22. How do you prevent users from claiming duplicate rewards due to retries, refreshes, or race conditions?
23. Explain how fairness is handled in your game economy and what telemetry would prove fairness.
24. How would you redesign reward distribution if users complain progression feels "too random"?
25. Walk through one reward claim transaction and all places where data consistency can break.
26. If leaderboard manipulation is attempted, how do you detect, prevent, and audit it?

## E) Spotify + Last.fm + External Dependencies

27. Explain BYO Spotify app flow and why it exists from both product and technical perspective.
28. How do you decide when to use user-specific tokens vs owner fallback mode, and what risks exist?
29. Explain how refresh tokens, expiration, and re-authentication are handled.
30. Walk through Last.fm verification logic and where false positives/false negatives can happen.
31. Explain your rate limiting design and why in-memory limiter is not enough at scale.
32. If Spotify API is up but Last.fm is partially down, how should product behavior degrade gracefully?

## F) Reliability, Observability, and Debugging

33. A user says "reward claimed but item missing." Give your full incident-debugging playbook.
34. API latency suddenly spikes in production. How do you isolate whether the bottleneck is DB, external API, or app logic?
35. Explain what logs, metrics, and traces you currently have and what is still missing.
36. If cron jobs miss runs for one day, how do you recover without duplicating rewards or quests?
37. Describe your idempotency strategy across game completion, claiming, and scheduled generation flows.
38. Which errors should fail fast vs retry silently, and why?

## G) Testing and Engineering Maturity

39. Explain your testing pyramid for ArmyVerse: what is unit-tested, what should be integration-tested, and what should be E2E.
40. Which single end-to-end test would you demo to prove system reliability to interviewers?
41. How do you test time-dependent logic like daily/weekly resets without flaky tests?
42. How would you create deterministic tests for reward and progression logic that normally depends on randomness?
43. If an interviewer asks "How confident are you in production correctness?", what evidence do you present?

## H) Reflection and Tradeoffs

44. What did you intentionally not build in ArmyVerse due to time constraints, and why?
45. Which design decisions do you now think were suboptimal, and what would you redesign first?
46. How did you balance shipping speed vs correctness as a fresher?
47. What performance optimization gave the highest impact and why?
48. If you had 14 days before deployment freeze, what exact sprint plan would you run?
