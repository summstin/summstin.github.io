---
title: "Code Review: Planning the enhancements"
excerpt: "A walkthrough of my Weight Tracker and Animal Shelter Dashboard code, using a code review checklist to find problems and plan the improvements."
collection: portfolio
---

## Overview

The code review was the first milestone of my CS-499 capstone. In a video of about 30 minutes, I walked through the existing code for my two artifacts, the Weight Tracker Android app (CS-360) and the Animal Shelter Dashboard (CS-340). I used the Code Review Checklist to look at each one in three categories: software design and engineering, algorithms and data structures, and databases. The goal was to find real problems in my own code and plan how to fix them.

**Video:** [Watch the code review]([[video link](https://youtu.be/boDEedYtrM0?si=yKYwYgYwOPtI15Y3)])

## What I found

**Weight Tracker**
- Passwords were stored in plain text
- Weights were not tied to a user, so every account could see the same entries
- One large screen class handled the interface, the database and the goal logic, and database work ran on the main thread
- Weights and dates were stored as text, and the goal check compared two strings, so a goal of `150` did not match an entry of `150.0`
- The goal check only fired when a new entry matched the goal exactly, so a user who went from 152 to 148 was never notified
- Entries had no ordering and no handling for empty lists, repeated dates or invalid numbers
- Every change read the whole table and rebuilt the whole screen

**Animal Shelter Dashboard**
- Database credentials were hardcoded, and I had to remove them to run the app locally

## What I planned

- **Software design:** a layered structure (ViewModel, repository, goal logic class, SMS notifier interface), coroutines, input validation, automated tests with a fake repository, and a fix for the SMS permission handling
- **Security:** PBKDF2-HMAC-SHA256 password hashing, with Argon2id as the stronger alternative, and a `user_id` on every weight entry with a real database migration so existing data is kept
- **Algorithms and data structures:** a cleaning step, a rolling average, goal-crossing detection and date-range summaries, with a benchmark comparing in-memory binary search to an indexed SQLite query
- **Databases:** removing the hardcoded credentials from the Animal Shelter Dashboard

## What I learned

Reviewing my own code with a checklist made me look for problems instead of defending what I had written. Several bugs, like the string comparison in the goal check, were things I had never noticed when I only tested that the app worked. Writing the plan also helped me connect the categories. For example, the date validation I planned for software design is what makes the algorithms work reliable.

**Code:** [View on GitHub](https://github.com/summstin/weight-tracker-android)
