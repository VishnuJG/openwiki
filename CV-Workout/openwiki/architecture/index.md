# Files

- [SQLite Profiles, Workout Records, and Statistics](persistence.md) - Documents how the desktop app locates its SQLite database, creates profile and per-user daily workout tables, and attempts to persist and display workout aggregates and angles. Highlights transaction boundaries, missing persistence implementations, and local-data and SQL-identifier risks.
- [Runtime Architecture and Ownership](runtime-ownership.md) - Maps the repository’s wired PySimpleGUI desktop flow, exercise and statistics modules, SQLite persistence boundary, and adjacent HTML and computer-vision artifacts. Highlights the missing persistence/statistics functions that currently interrupt the apparent end-to-end path.
