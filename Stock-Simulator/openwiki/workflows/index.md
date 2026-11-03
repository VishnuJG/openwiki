# Files

- [Current Application Startup and Login UI](current-login.md) - Traces the active Tkinter startup from `src/app.py` through `StockSimulator` construction and the event loop. Documents the current submit callback, including its credential-printing behavior and the absence of authentication or database calls.
- [Standalone Trading and Account Prototypes](prototype-trading.md) - Explains the separate root-level Tkinter prototypes for PostgreSQL account access, holdings, wallet changes, and buy/sell operations, including their database effects and failure boundaries. Distinguishes this prototype behavior from the active src application and highlights unsafe SQL and partial-commit risks.
