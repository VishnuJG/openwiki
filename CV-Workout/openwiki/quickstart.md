---
type: concept
title: Agent Quickstart
description: Routing hub for locating the active desktop workout workflow, pose counting, SQLite persistence, and local setup guidance. Summarizes verified source behavior and flags important runtime and validation gaps before you change or launch the app.
tags: [quickstart, navigation, desktop-app, operations]
verified:
  - by: openwiki/0.7.2
    at: 2026-11-03T07:26:57.360Z
sources:
  - id: openwiki-source-5b4d5d72cdd0d7ccf8b55978
    resource: repo://code/index.html
  - id: openwiki-source-31e240a3b383d7e1d1c67f9b
    resource: repo://noFrontEnd/dbFuns.py
  - id: openwiki-source-e9c17c887d1cec39fc7ed4e4
    resource: repo://noFrontEnd/entry.py
  - id: openwiki-source-1444bf33fc866062916f0780
    resource: repo://noFrontEnd/front.py
  - id: openwiki-source-f5c193dd46dd8eddeee59038
    resource: repo://noFrontEnd/legcurls.py
  - id: openwiki-source-07c9228d248ab461ed3bf9f3
    resource: repo://noFrontEnd/reps.py
  - id: openwiki-source-107a6a0612abef02e70bd754
    resource: repo://noFrontEnd/stats.py
  - id: openwiki-source-23775c3de52f3ab95a13cb8b
    resource: repo://README.md
  - id: openwiki-source-373640cd8a0886cee69db282
    resource: repo://requirements.txt
generated: { by: "openwiki/0.7.2", at: "2026-11-03T07:26:57.360Z" }
---

# Agent Quickstart

Use this page to choose the owning workflow, not as a source-tree catalog. The repository describes itself as a computer-vision workout capstone, but its README has no launch instructions; the code paths below establish what the app does. ([README](../README.md#L1-L3))

## Route by problem

| If you are working on… | Start here | Why |
|---|---|---|
| Login, account creation, disclaimer, or exercise selection | [Login-to-workout workflow](workflows/workout-session.md) and [`entry.py`](../noFrontEnd/entry.py#L9-L13) | This is the desktop path: it checks the database, handles login/new-user actions, requires disclaimer agreement, then opens the exercise window. |
| Rep counting, joint angles, camera loop, or thresholds | [Pose angles and rep counting](concepts/pose-counting.md) | The selected exercise calls the arm-rep or leg-curl pose loop, which returns left/right counts and angle measurements. |
| Missing results, daily totals, or charts | [SQLite persistence](architecture/persistence.md) | Exercise results and angle fields go through SQLite helpers; chart requests also read through that persistence boundary. See the known missing-helper caveat below. |
| Launch, imports, camera, database path, or runtime diagnostics | [Local development](operations/local-development.md) and [runtime dependencies](integrations/runtime-dependencies.md) | Setup is not fully specified in the README or dependency file, so verify the local environment rather than assuming a reproducible install. |
| Deciding which application or files to change | [Runtime ownership](architecture/runtime-ownership.md) | The desktop app is under `noFrontEnd/`; adjacent `code/` assets include a template dashboard and prototype scripts, not the demonstrated desktop entry flow. |

## Active flow at a glance

The active source path begins at `noFrontEnd/entry.py`: account handling leads to the disclaimer, and accepted users reach `frontdef`. From there the user chooses Reps or Leg Curls; each exercise returns counts and angle values that the front end sends to database helpers. Stats launches a separate chart window. ([Entry](../noFrontEnd/entry.py#L28-L69), [exercise routing and persistence calls](../noFrontEnd/front.py#L54-L87), [statistics window](../noFrontEnd/stats.py#L19-L38))

## Important caveats before changing or running

- **The apparent startup health warning is unconditional in normal operation.** `dbHealthCheck()` creates `Profiles` if needed, but returns no success value; `entry.py` treats its `None` result as false and displays “Database is not set up correctly” even after that check. ([database check](../noFrontEnd/dbFuns.py#L9-L20), [caller](../noFrontEnd/entry.py#L9-L12))
- **Some post-exercise and stats paths reference helpers absent from the inspected persistence module.** `front.py` calls `dbUpdatefunsetting` after an exercise, and `stats.py` calls `dbgetinfo` when a chart is selected; neither is defined in `noFrontEnd/dbFuns.py`. Exercise selection can therefore fail after the count update, while chart selection cannot complete as written. Verify or repair these interfaces before treating results or charts as working. ([calls](../noFrontEnd/front.py#L57-L80), [stats read call](../noFrontEnd/stats.py#L37-L70), [available database helpers](../noFrontEnd/dbFuns.py#L1-L60))
- **Database location and setup are fragile.** The SQLite filename is built from `os.getcwd()` plus a literal backslash and `test.db`, rather than a platform-independent joined path; launching from another working directory changes the database location. New accounts create a profile and a per-user stats table. ([path and schema setup](../noFrontEnd/dbFuns.py#L5-L20), [account persistence](../noFrontEnd/dbFuns.py#L52-L60))
- **Do not infer a complete installation recipe from `requirements.txt`.** It lists only `mediapipe` and `cv2`, while the desktop modules also import PySimpleGUI, NumPy, and Matplotlib; the README provides no launch command. Dependency resolution, GUI startup, SQLite behavior, and camera access need local verification. ([requirements](../requirements.txt#L1-L2), [desktop imports](../noFrontEnd/entry.py#L1-L7), [exercise imports](../noFrontEnd/reps.py#L1-L4), [statistics imports](../noFrontEnd/stats.py#L1-L6), [README](../README.md#L1-L9))

`code/index.html` extends a base template, uses template tags, and references a `getvid` URL; it is a web-template asset, not evidence that the desktop login/workout flow is backed by a running web app. Start ownership investigations with `noFrontEnd/` unless a task specifically concerns that experimental/template material. ([dashboard template](../code/index.html#L1-L8), [template URL and dashboard content](../code/index.html#L49-L63))

The statements above are based on source inspection, not a successful local launch or end-to-end test. The repository search for focused test files found none; verify imports, database initialization, GUI, and webcam behavior in the target environment before relying on runtime behavior.
