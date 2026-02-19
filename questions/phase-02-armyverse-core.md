# Phase 02 - ArmyVerse Core Questions (Basics to Early Intermediate)

## A) Product and Scope Understanding

1. What problem does ArmyVerse solve for BTS fans?
2. Which 3 modules are most important in ArmyVerse today?
3. Who is the primary user persona for ArmyVerse?
4. How would you describe ArmyVerse architecture in under 2 minutes?
5. Why did you choose a single Next.js app instead of separate frontend/backend repos?
6. Which features are read-heavy vs write-heavy in ArmyVerse?
7. What are the critical user journeys in the app?
8. How do you prioritize features in a fan-platform product?
9. What tradeoff did you make between feature richness and maintainability?
10. If users drop off after signup, what data would you inspect first?

## B) Next.js and API Layer

11. Why did you use Next.js 14 App Router?
12. What is the difference between server components and client components in your app?
13. How do Next.js route handlers work in your project?
14. How are API routes organized by domain in ArmyVerse?
15. Why use `force-dynamic` in some API routes?
16. How do you manage runtime differences (`edge` vs `nodejs`)?
17. What are common pitfalls in Next.js API route error handling?
18. How do you standardize API response shapes across modules?
19. How do you handle retries and partial failures on the frontend?
20. How do you protect private API routes from unauthorized access?

## C) Database and Models

21. Why did you choose MongoDB + Mongoose for ArmyVerse?
22. Explain your model organization strategy in `lib/models`.
23. How do you avoid schema drift in fast-moving projects?
24. Which entities are core to gameplay state?
25. How do you model one-to-many relationships in your design?
26. How do you index frequently queried collections?
27. Where do you use TTL or time-based cleanup?
28. How do you prevent duplicate writes in reward flows?
29. How do you handle migrations when schema fields evolve?
30. How would you backfill historical data safely?

## D) Authentication and Security Basics

31. Why did you support both Firebase auth and custom JWT auth?
32. How does unified auth verification work in your app?
33. How do you extract and verify bearer tokens in route handlers?
34. What are security risks of storing JWT in localStorage?
35. Why is `JWT_SECRET` mandatory and what happens if missing?
36. How do you hash passwords and why bcrypt cost factor matters?
37. How do you validate signup/signin payloads?
38. How do you enforce username/password quality rules?
39. How do you handle auth fallback when Firebase Admin is not configured?
40. What extra hardening would you add if this app scaled to enterprise usage?

## E) Spotify and Playlist Features

41. What playlist generation inputs do you collect from users?
42. How does AI playlist generation flow work end-to-end?
43. How do you resolve generated songs to real Spotify track IDs?
44. Why keep both manual playlist and AI playlist flows?
45. How does export-to-Spotify work at high level?
46. Why does ArmyVerse support owner fallback mode for exports?
47. What is the purpose of BYO Spotify app mode?
48. What is PKCE and where is it applied in your flow?
49. Which scopes are required for playlist creation and why?
50. How do you handle expired Spotify tokens in the UX?

## F) Game System Basics

51. Explain quiz flow from question fetch to reward grant.
52. How do you score quiz performance and convert it to XP?
53. Why is card drop XP-gated in quiz flow?
54. What is the role of `UserGameState` in your system?
55. How are daily limits enforced for quiz starts?
56. What is Borarush and how is XP computed from turns?
57. How do mastery levels work in broad terms?
58. How do daily and weekly quest period keys work?
59. How is leaderboard score updated across daily/weekly/all-time?
60. How do you avoid leaderboard inconsistencies in concurrent updates?

## G) Content and Community Features

61. Why did you build a Tiptap-based blog editor?
62. What is stored for a blog post besides title and content?
63. How do you sanitize user-generated content before storing/displaying?
64. How do nested comments or reactions affect data modeling?
65. How do you handle image uploads and media hosting?
66. What is your strategy for read-time or metadata computation?

## H) Cron, Integrations, Testing, Deploy

67. Which tasks run on cron and why are they scheduled?
68. How do you secure cron endpoints from unauthorized triggers?
69. Why does Last.fm integration need caching and throttling?
70. What is your current test strategy and biggest testing gap in ArmyVerse?
71. What deployment setup do you use and what environment variables are critical?
72. If production breaks after deploy, how do you triage quickly?
