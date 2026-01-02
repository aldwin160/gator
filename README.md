# gator
Boot.dev Typescript project to create a small CLI blog aggregator that fetches RSS feeds, stores feed metadata in Postgres, and allows users to create accounts and follow feeds.

## Quick links
- **Usage:** see `docs/USAGE.md` for CLI examples and command reference.
- **Developer docs:** see `docs/DEVELOPMENT.md` for setup, architecture, and contribution notes.

## Quickstart
1. `npm ci`
2. Create `~/.gatorconfig.json` with `db_url` and `current_user_name` (see `docs/DEVELOPMENT.md`).
3. `npm run migrate` to apply DB migrations.
4. Run commands: `npm start -- <command> [args...]`

---

For details about internals, adding commands, and database migrations, read `docs/DEVELOPMENT.md`.