# giong db

> giong 1.5.0 · https://giong.run/docs/cli/db/ · source: https://github.com/giong-run/Giong/blob/v1.5.0/giong-cli/README.md#giong-db

```sh
giong db up            # a development PostgreSQL, MySQL or Redis in Docker or Podman
giong db new add_users # migrations/0002_add_users.sql
giong db migrate       # apply what is new; giong db status shows what is applied
giong db down          # stop it; --volumes also deletes the data
```

Starts the databases the project depends on in containers, with a random password, a free port and a volume, and writes `DATABASE_URL` or `REDIS_URL` into `.env` (owner-only, git-ignored). Each checkout gets its own container, named after the package, the engine and a short hash of the project's path (`giong-shop-postgres-3fa2c1`), so two copies of a project never share a database. The container is recorded in `.giong/processes.json`: `giong dev status` lists it and `giong dev stop` stops it, keeping its data. Migrations are forward-only SQL files `migrations/NNNN_name.sql`; each PostgreSQL file runs in one transaction (unless its first line is `-- giong:no-transaction`), each applied file is recorded with its SHA-256 checksum, a changed applied file is refused and a failing one names its line. With a `DATABASE_URL` of your own, `migrate` and `status` work on that database. `giong db reset --yes` empties a database `giong db up` started.
