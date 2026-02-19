# Cross-Project + Outside-Resume Long-Form Questions

These are high-value long-answer questions interviewers ask to test depth, ownership, and engineering maturity.

## A) Comparing ArmyVerse and DocBuilder

1. Compare ArmyVerse and DocBuilder architectures: where they are similar, where they are very different, and why.
2. Which project is stronger for demonstrating backend engineering and which is stronger for AI workflow engineering?
3. If you had to merge both into one platform, what shared infrastructure would you build first?
4. Which project has higher security risk surface and why?
5. Which project has more scaling risk at 100k users and what exact bottleneck appears first?
6. How do your testing strategies differ between both projects and what does that reveal?
7. What common engineering principles did you apply in both projects?
8. Which project shows better product-market potential and what evidence supports your view?

## B) System Design for Fresher Interviews

9. Design a generic "job queue + worker + retry + dead-letter" architecture and explain how both projects can use it.
10. Explain how you would design an event-driven architecture for reward claims and document generation updates.
11. Show how you would add caching layers (API, DB query, computed result) and when each layer is useful.
12. Explain idempotency and exactly where you would enforce it in both projects.
13. Design a production-grade rate-limiting strategy for user APIs and third-party APIs.
14. Explain multi-tenant design basics and how your current projects would evolve for enterprise accounts.
15. Walk through a blue-green or canary deployment strategy for minimizing production risk.
16. How would you design observability dashboards that leadership can use, not just engineers?

## C) Production Reliability and Incident Handling

17. Describe a full incident response playbook for a Sev-1 outage in either project.
18. How do you decide rollback vs hotfix under real-time production pressure?
19. Explain SLO, SLA, and error budget in simple terms and how you would define them for your projects.
20. If third-party providers fail (Spotify/Groq/Search), how do you degrade gracefully while keeping trust?
21. How do you run postmortems so that the same class of bug does not repeat?
22. How do you prioritize reliability work when product team pushes for fast feature delivery?

## D) Security and Compliance Basics (Outside Resume but Expected)

23. Explain OWASP Top 10 risks most relevant to your projects and how you mitigate each.
24. Describe a secret rotation and incident containment process if API keys are leaked.
25. Explain audit logging: what to log, what never to log, and how to handle PII safely.
26. How would you prepare your projects for basic compliance expectations (SOC2-like mindset)?
27. How do you secure CI/CD pipeline and prevent secret leakage in logs/build artifacts?
28. Explain zero-trust mindset in practical terms for small teams.

## E) Databases and Data Consistency

29. Compare transaction/consistency expectations in gaming workflows vs AI content workflows.
30. Explain eventual consistency with real examples from your projects.
31. How do you model schemas for future growth when requirements are still changing weekly?
32. How do you backfill or migrate data safely in production without breaking APIs?
33. What backup and restore strategy would you define before launch?
34. How would you detect and repair data corruption or unexpected duplicates?

## F) Leadership, Ownership, and Career-Stage Questions

35. As a fresher, how do you decide what "good enough for shipping" means?
36. Give an example where you made a wrong technical assumption and corrected it.
37. How do you break down large uncertain tasks when requirements are incomplete?
38. How do you communicate tradeoffs to non-technical stakeholders without oversimplifying?
39. How do you estimate effort more accurately after your first estimate goes wrong?
40. If hired, what type of mentorship or team setup helps you deliver fastest?

## G) Explain-Only Rounds (No Coding)

41. Explain one complex bug from each project using: symptom -> hypothesis -> test -> fix -> prevention.
42. Explain one architecture decision you are proud of and one you regret, with technical reasoning.
43. Explain how you would onboard a new engineer to each codebase in 1 day.
44. Explain how you would defend these projects if interviewer says "This feels too ambitious for a student."
45. Give your final "project defense pitch": 6 minutes, technical but simple, high confidence, no over-claiming.
