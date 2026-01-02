# Development Guide

## Overview 🔧
This document describes how the project is organized, how to run it locally, how the core parts work, and notes for contributors.

**Project purpose:** A small CLI blog aggregator that fetches RSS feeds, stores feed metadata in Postgres and allows users to register, add feeds, and follow feeds.

---

## Layout / Key files 📁
- `src/index.ts` — CLI entry point. Registers handlers and dispatches commands.
- `src/commands/` — Command handlers (login/register/users, feeds, feed-follows, aggregate, reset).
- `src/middleware.ts` — middleware helpers (e.g., `middlewareLoggedIn` to require a logged-in user).
- `src/config.ts` — Reads and writes `~/.gatorconfig.json` (db URL and current user).
- `src/lib/rss.ts` — RSS fetch & parse utility.
- `src/lib/db/` — Database layer
  - `schema.ts` — Drizzle schema definitions (users, feeds, feed_follows)
  - `queries/` — Query helpers for each table
  - `migrations/` — SQL migration files
- `drizzle.config.ts` — Drizzle configuration (used by `drizzle-kit`).

---

## Setup / Local development 🛠️
Prerequisites:
- Node.js (18+ recommended)
- Postgres database
- `npm` (or compatible package manager)

Steps:
1. Install deps: `npm ci`
2. Create a Postgres database and note the connection URL (eg. `postgres://user:pass@localhost:5432/gator_dev`).
3. Create a config file in your home directory: `~/.gatorconfig.json` with the shape:

```json
{
  "db_url": "postgres://user:pass@localhost:5432/gator_dev",
  "current_user_name": ""
}
```

4. Run migrations: `npm run migrate` (this uses `drizzle-kit migrate` configured in `package.json`).
5. Start the CLI: `npm start -- <command> [args...]`

Note: Use `npm start --` to pass arguments to the underlying `tsx ./src/index.ts` process.

---

## Database / Migrations 📦
- Migrations are in `src/lib/db/migrations/` and created by `drizzle-kit`.
- Use `npm run generate` to regenerate migration snapshots when schema changes, and `npm run migrate` to apply migrations to the database.
- The runtime reads `db_url` from `~/.gatorconfig.json`.

---

## How it works — high level 🧭
- The CLI entry (`src/index.ts`) builds a `CommandsRegistry`, registers handlers and executes a command based on `process.argv`.
- Commands are plain async functions conforming to `CommandHandler` or `UserCommandHandler` types (see `src/commands/commands.ts`).
- To require an authenticated user, wrap a `UserCommandHandler` with `middlewareLoggedIn` which will check `~/.gatorconfig.json` and load the user.
- DB access uses `drizzle-orm` with simple queries in `src/lib/db/queries/`.
- RSS fetching/parsing is implemented in `src/lib/rss.ts` using `fast-xml-parser`.

---

## Common developer tasks ✅
- Add a new command:
  1. Create a handler in `src/commands/` using the existing pattern.
  2. Export the handler and register it in `src/index.ts` using `registerCommand`.
  3. If the command needs a logged-in user, wrap the handler in `middlewareLoggedIn`.

- Add a DB table:
  1. Update `src/lib/db/schema.ts` with a new table using Drizzle helpers.
  2. Run `npm run generate` to refresh Drizzle snapshots (if used).
  3. Add a migration file or use `drizzle-kit` to generate SQL for the change.
  4. Run `npm run migrate` to apply it.

- Tests: There are no tests in the repo yet — add unit tests around handlers and query functions if required.

---

## Notes & gotchas ⚠️
- `~/.gatorconfig.json` is the single source of runtime configuration—do not commit your local configs.
- `handlerAgg` currently has a hardcoded feed URL for demonstration.
- `reset` command deletes all users (and cascades to feeds) — use with caution.

---

## Contributing 💡
- Open a PR with a clear description and include tests for new behavior when possible.
- Keep changes small and focused; update the docs and migrations where needed.

---

If you want, I can add an architecture diagram or an example workflow (end-to-end) — tell me which you'd prefer next.