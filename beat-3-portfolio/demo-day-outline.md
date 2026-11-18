# Demo Day outline (about 5 minutes, ungraded)

Present from technical-writeup.md. One beat per section; show the proof blocks on screen.

1. **Hook (30s):** "I built a tool to find my first open source issue. Its first run picked the wrong ones." Show the React Native #1 excerpt.
2. **What it is (45s):** issue-scout = search strategy (mine) → free ranking → paid grading against my rubric (mine). About $0.20 per graded issue.
3. **What went wrong (60s):** 9 of 11 picks already taken; same-day issues "failed" responsiveness. Show the `maintainer-responsive` fail line.
4. **What I changed (60s):** `-linked:pr` in search; the before/after rubric quote; unlinked-PR rule.
5. **Result (45s):** found conversations#911; scout said reject (spec not settled); I agreed with why, and turned it into the question in my claim comment. Show the comment quote.
6. **Where it stands + next (45s):** no reply yet; fixes not re-verified; next: canary rescan, triage-bot rule, follow up on #911.
7. **One lesson (15s):** your own sentence.

Likely questions to prepare:
- Why take an issue your own tool rejected?
- How do you know the rubric fixes work if you didn't rerun the scan?
- What will you do if the maintainer never answers?
