# CLAUDE.md

This file gives Claude Code (and anyone else picking up this project) the context needed to work on **ExpenseFlow** correctly.

## 1. Project Overview

ExpenseFlow is a **proof of concept (PoC)** for an expense submission system. At its core, it is an API — meaning other programs (or a command line, not a visual app) talk to it — that lets someone submit an expense, automatically converts the amount into Indian Rupees, saves it, and uses AI to generate a short spending insight about that submission.

This is a small, focused PoC. It is intentionally *not* trying to be a full expense management product yet — see [Out of Scope](#5-out-of-scope) below.

## 2. What It Does

- Accepts an **expense submission** (e.g. amount, currency, description).
- **Stores** the expense.
- **Normalises the amount to INR** (Indian Rupees), in **minor units** — meaning paise, not rupees. For example, ₹150.50 is stored as `15050`. This avoids rounding/decimal errors, which is a common source of bugs in financial software.
- Generates **one AI-produced spending insight** per submission — a short piece of commentary or observation about that expense (e.g. "This is unusually high for a meal expense").

## 3. Who It's For

- **Primary user:** A finance professional who submits and approves expenses.
- **How they interact with it:** Through a **CLI** (command line interface) — i.e. by typing commands into a terminal, not by clicking through a website or app. There is no visual interface in this PoC.

## 4. Success Criteria

The PoC is considered successful when, for a given submission:

1. The expense is stored.
2. The amount is correctly stored in **INR minor units** (paise).
3. Exactly **one AI insight** is generated for that submission.
4. The **CLI output** clearly shows the result of the submission (stored expense + insight).

## 5. Out of Scope

To keep this PoC focused, the following are explicitly **not** part of it:

- **No UI** — no website, no app, no visual interface of any kind.
- **No email** — no notifications, receipts, or alerts sent by email.
- **No Postgres** — this PoC does not use a Postgres database (a lighter-weight or simpler storage approach is expected instead).
- **No multi-currency UI** — while amounts may be normalised to INR internally, there is no interface for handling or displaying multiple currencies.

If a request or idea falls into one of these categories, it should be treated as a future enhancement, not part of this PoC's deliverable.

## 6. Technical Stack

- **Language: Python** — the whole application is written in Python.
- **Database: SQLite** — a lightweight, file-based database. Unlike Postgres or MySQL, it doesn't need a separate server running; it just saves everything into a single file on disk. This fits well with the "no Postgres" scope boundary and keeps the PoC simple to run.
- **Testing: pytest** — the standard Python library for writing and running automated tests. Tests should be added alongside features to check that expenses are stored correctly, amounts are normalised to INR minor units correctly, and an insight is generated per submission.

**Why this matters:** These choices keep the PoC easy to set up (no external services to install or configure) and easy to verify (automated tests confirm the success criteria in [Section 4](#4-success-criteria) actually hold, not just that the code runs).

## 7. Key Terms (Plain-English Glossary)

- **API** — a way for software to talk to other software, without a visual interface.
- **CLI (Command Line Interface)** — a text-based way of running the program, by typing commands into a terminal.
- **Normalise (amount)** — convert an amount from its original currency into a single standard currency (here, INR) so everything can be compared consistently.
- **Minor units** — the smallest unit of a currency, used to avoid decimal rounding errors. For INR, the minor unit is paise (1 rupee = 100 paise).
- **Insight** — a short, AI-generated observation or comment about a submitted expense.
- **PoC (Proof of Concept)** — a small, working version built to test whether an idea works, not a finished product.
- **SQLite** — a lightweight database that stores everything in a single file, with no separate server to run.
- **pytest** — a Python tool that automatically runs a set of checks ("tests") against the code to confirm it behaves as expected.

## 8. Working Notes for Claude Code

- Keep changes scoped to the brief above. Don't add UI, email, Postgres, or multi-currency handling unless the user explicitly asks to expand scope.
- Since the primary (and only) interface is the CLI, prioritise clear, readable CLI output when implementing or changing features — that output is how success is verified.
- Build the application in **Python**, store data in **SQLite**, and write automated tests with **pytest** for each feature (see [Section 6](#6-technical-stack)).
- The user who owns this project is **not a technical person**. When explaining code, architecture, or decisions back to them, use plain language and avoid unexplained jargon.
- When in doubt about a design decision not covered by the brief, ask rather than assume — this is a PoC with a specific, narrow success bar.
