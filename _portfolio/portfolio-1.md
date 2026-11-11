---
title: "Weight Tracker: Android app with software design enhancements"
excerpt: "A Kotlin/SQLite Android app I rebuilt to hash passwords, separate each user's data, and run on a tested, layered design."
collection: portfolio
---

## Overview

The Weight Tracker is an Android app that lets users create an account, track their weight, set a goal weight, and receive a text message when they reach their goal. I originally created the app for CS-360 and revisited it for my CS-499 capstone. I used this project to fix issues I found during my code review and improve how the app works behind the scenes.

## What I did

- Built in **Kotlin** with **SQLite**, using **Android Studio**
- Replaced plain-text passwords with **salted PBKDF2-HMAC-SHA256** hashing
- Fixed a security issue that allowed users to see other users' weight entries by making sure records are tied to the correct account
- Added a **database migration** so existing user data is kept when the app is updated
- Split a large screen class into smaller components, including a ViewModel, services, and a repository
- Moved database operations off the main thread using coroutines
- Added input validation and wrote 32 unit tests to check app functionality and make sure users cannot access or modify someone else's records

## What I learned

Working on this project showed me how changes in one part of an app can affect everything else. For example, changing how passwords are stored also meant updating the database and making sure existing user data was not lost. I also got a better understanding of how separating code into smaller components makes an app easier to maintain and test.

**Code:** [View on GitHub](https://github.com/summstin/weight-tracker-android)
