# giong new

> giong 1.5.5 · https://giong.run/docs/cli/new/ · source: https://github.com/giong-run/Giong/blob/v1.5.5/giong-cli/README.md#giong-new

```sh
giong new <dir> [--template web|grpc|worker] [--db sqlite|postgres|mysql|none[,redis]] [--tls] [--name NAME] [--no-git] [--no-ai] [--dry-run]
```

Writes a project that builds, passes `clippy -D warnings` and passes its tests from the first minute, then runs `git init` and `giong ai init` in it. Every project has one worker per core with graceful shutdown on `SIGTERM`, giong-mcp (off until `GIONG_MCP` is set), an end to end test, `.env.example`, a Dockerfile, a GitHub Actions workflow with the database services it needs, and a dev profile that builds faster: on a PostgreSQL, Redis and TLS service a clean debug build takes 12.6 s instead of 15.3 s and the target directory 296 MB instead of 640 MB (Rust 1.100, [method](https://github.com/giong-run/Giong/tree/main/bench/results/devkit/template-profile)).

| Template | What it is |
|---|---|
| `web` (default) | A JSON API on giong-web with `/health` (and `/ready` when it has a database): a users API on SQLite by default, or on PostgreSQL or MySQL with `--db` (with Redis as a cache when `redis` is added), counters on Redis (`--db redis`), or a hello service without a database (`--db none`). Migrations in `migrations/`. |
| `grpc` | A gRPC service from `proto/service.proto` with health and reflection, built by giong-grpc-build (no `protoc` to install). |
| `worker` | A job runner: jobs claimed from a table with `FOR UPDATE SKIP LOCKED` (PostgreSQL) or in a transaction (MySQL), from a Redis list, or on a timer, with retries and backoff. |

SQLite needs nothing installed or started: the database is the file `data/<name>.db`, made at the first start, compiled in through rusqlite (feature `bundled`). Its calls block, so the project's `src/db.rs` runs each one on giong's blocking lane (`giong::spawn_blocking`), with one write connection and a few read connections in WAL mode shared by every worker; the tests use a file of their own each, and the CI workflow starts no service. One copy of the service owns a file; for several machines start from `--db postgres`.

`--tls` serves HTTPS or gRPC over TLS when `TLS_CERT` and `TLS_KEY` are set. `sqlite`, `postgres` and `mysql` exclude each other, `grpc` takes no database and `worker` one, without `--tls`: anything else is refused with `GIONG-E5032` and the choices that work. The directory must be new or empty (`GIONG-E5031`); a write that fails takes back everything it made. Every combination is built, linted, tested on io_uring and on epoll against real databases, started and stopped with `SIGTERM` on every pull request.
