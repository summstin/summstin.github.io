---
title: "Weight Tracker: Algorithms and data structures enhancements"
excerpt: "I added goal-crossing detection, a rolling average and date-range summaries to my Android weight tracker, then measured two ways to answer range queries."
collection: portfolio
---

## Overview

The Weight Tracker is an Android app that lets users track their weight, set a goal weight, and receive a text message when they reach their goal. I originally created it for CS-360. For the second enhancement of my CS-499 capstone, I built on my software design version to improve the algorithms and data structures behind it. My code review found that the app only noticed a goal if a new entry matched it exactly, never looked at a user's history, and rebuilt everything from the database on every action.

## What I did

- Wrote a **data cleaning step** that skips and reports invalid entries, keeps only the newest entry when a date repeats (using a HashMap), and sorts by real dates in O(n log n)
- Added a **rolling average** that keeps a running sum instead of recalculating each window, so it runs in O(n) instead of O(n·k)
- Built **goal-crossing detection** that compares each pair of consecutive entries to the goal, so a user who goes from 152 to 148 is now detected when the goal is 150. It handles starting at the goal, staying at it, and crossing more than once
- Added **date-range summaries** using a sorted list and **binary search**, which takes O(log n + k) per query
- Added a summary line to the main screen showing the 7-entry average and the last 30 days
- Wrote 17 new unit tests covering empty data, repeated dates, invalid values, and goals above and below the starting weight
- Wrote an instrumented **benchmark** comparing the in-memory binary search with an indexed SQLite query at 1,000, 10,000 and 100,000 entries

## Benchmark results

| Entries | Memory setup | Memory per query | SQLite per query |
|---|---|---|---|
| 1,000 | 72.1 ms | 54.6 µs | 107.7 µs |
| 10,000 | 42.8 ms | 16.0 µs | 67.2 µs |
| 100,000 | 363.1 ms | 14.7 µs | 83.1 µs |

These were measured on an emulator, so the ratios matter more than the exact times. The in-memory approach was faster per query, but its setup cost only pays off after roughly 840 to 5,300 queries. The app rebuilds its data on each reload and asks one question, so SQLite's indexed query is the better fit for how the app is actually used.

## What I learned

The biggest lesson was that a faster algorithm is not always the better choice. Binary search won every per-query comparison, but the benchmark showed its setup cost matters when the data is rebuilt each time. I also learned that edge cases are design questions: deciding what "reaching the goal" means for a first entry, or an entry added for an older date, changed how I wrote the logic and my tests. Keeping the analytics code free of Android classes made it quick to test on its own.

**Code:** [View on GitHub](https://github.com/summstin/weight-tracker-android/tree/algorithms-data-structures)
