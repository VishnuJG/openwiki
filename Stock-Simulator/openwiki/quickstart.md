---
type: agent guide
title: Agent Quickstart
description: Minimal local startup for the active Tkinter application, with its entrypoint and current login behavior. Use the routing map to find the owning workflow, architecture, persistence, security, operations, and testing documentation.
tags: [quickstart, agent-guide, tkinter, routing]
verified:
  - by: openwiki/0.7.2
    at: 2026-11-03T08:50:35.037Z
sources:
  - id: openwiki-source-aa86364bf475feb81d09a70f
    resource: repo://doc1.py
  - id: openwiki-source-8ce0b8a70bfb88730d986a88
    resource: repo://src/app.py
  - id: openwiki-source-84038e7eed1664f4482dad07
    resource: repo://src/login.py
  - id: openwiki-source-202c9508202ec614edc8ed6e
    resource: repo://src/README.md
generated: { by: "openwiki/0.7.2", at: "2026-11-03T08:50:35.037Z" }
---

# Agent Quickstart

## Start the active application

From the repository root, run:

```sh
cd src
python app.py
```

This is the active Tkinter entrypoint: `app.py` creates the root window, constructs `StockSimulator`, and starts Tkinter's event loop ([`src/app.py`](../src/app.py#L1-L7)). The source README identifies Python 3.x, Tkinter, and `psycopg2` as requirements and says to run `app.py` ([`src/README.md`](../src/README.md#L12-L18)). The login module imports `psycopg2`, so it must be importable even though this UI path does not open a database connection ([`src/login.py`](../src/login.py#L1-L3), [`src/login.py`](../src/login.py#L26-L35)). A working local Tk display is also needed to show the window.

```mermaid
flowchart TD
    A["Run python app.py from src"] --> B["Create Tk root"]
    B --> C["Construct StockSimulator"]
    C --> D["Build login form"]
    D --> E["Enter Tk event loop"]
    E --> F["Submit calls login callback"]
    F --> G["Print entered email and password"]
```

The diagram shows the active UI startup and submit path; the callback currently prints the entered values rather than authenticating or querying PostgreSQL ([`src/app.py`](../src/app.py#L4-L7), [`src/login.py`](../src/login.py#L12-L30)).

> **Important:** The visible login form is not an implemented authentication boundary. Its Submit callback prints the email and password to stdout. Do not use real credentials with it; see [Authentication, Sensitive Data, and Trust Boundaries](security/auth-and-secrets.md).

## Route by task or symptom

| If you are investigating… | Start here |
| --- | --- |
| Which modules own active app behavior, and how that differs from root-level scripts | [System Architecture and Ownership](architecture/ownership.md) |
| Tkinter startup, login form, or Submit behavior | [Current Application Startup and Login UI](workflows/current-login.md) |
| Account, holdings, buy, or sell logic in standalone scripts | [Standalone Trading and Account Prototypes](workflows/prototype-trading.md) |
| PostgreSQL connection behavior, wrapper semantics, or prototype queries | [PostgreSQL Integration and Persistence](integrations/postgresql.md) |
| Credentials, authentication gaps, or sensitive data handling | [Authentication, Sensitive Data, and Trust Boundaries](security/auth-and-secrets.md) |
| Local dependencies, startup failures, or runtime triage | [Local Runtime, Configuration, and Failure Triage](operations/local-runtime.md) |
| How to validate a change and what test coverage exists | [Testing and Change Validation](testing/validation.md) |

The current `src/app.py` → `StockSimulator` path is separate from the root-level prototype programs; for example, `doc1.py` creates its own Tk root and imports PostgreSQL directly ([`src/app.py`](../src/app.py#L1-L7), [`doc1.py`](../doc1.py#L1-L5)). Treat the prototype workflow and its persistence behavior as a separate concern, not as functionality reached through the active login screen.
