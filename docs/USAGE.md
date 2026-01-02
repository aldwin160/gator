# Usage Guide (CLI)

Quick reference for using the `gator` CLI.

## Quickstart 🚀
1. Ensure you have run migrations and have a populated `~/.gatorconfig.json` (see `docs/DEVELOPMENT.md`).
2. Run commands using:

```bash
npm start -- <command> [args...]
```

Examples:

- Register a new user and auto-switch to it:

```bash
npm start -- register alice
# -> "User created successfully!"
```

- Switch to an existing user:

```bash
npm start -- login alice
# -> "User switched successfully!"
```

- List users:

```bash
npm start -- users
# -> "* alice (current)""
```

- Add a feed (requires logged-in user):

```bash
npm start -- addfeed "Tech Blog" "https://example.com/feed.xml"
```

- List all feeds:

```bash
npm start -- feeds
```

- Follow a feed (requires logged-in user):

```bash
npm start -- follow "https://example.com/feed.xml"
```

- List your follows (requires logged-in user):

```bash
npm start -- following
```

- Unfollow a feed:

```bash
npm start -- unfollow "https://example.com/feed.xml"
```

- Run an example aggregate (demo):

```bash
npm start -- agg
# prints parsed RSS JSON
```

- Reset database (deletes all users):

```bash
npm start -- reset
# -> "Database reset successfully!"
```

## Commands reference 📝
- `register <name>` — create a new user and set them as current
- `login <name>` — switch the current user
- `users` — list users (shows current)
- `addfeed <feed_name> <url>` — create a new feed and subscribe the current user
- `feeds` — list all feeds
- `follow <feed_url>` — current user follows an existing feed
- `following` — list feeds followed by current user
- `unfollow <feed_url>` — unfollow a feed
- `agg` — demo RSS fetch + print
- `reset` — wipe users table (use carefully)


If you'd like, I can add sample session outputs or a script for populating demo data.