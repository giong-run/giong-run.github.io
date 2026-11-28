# Get started with giong

> giong 1.5.7 · https://giong.run/docs/

A running service in a few commands, then every page of the documentation: the `giong` command, a page for each crate, the best practices, the agent skill. The API reference for each crate is on docs.rs.

## From zero to a running service

The `giong` command writes a project that builds and passes its tests, runs it on localhost with a rebuild on every save, and checks it the way an agent can read. One project, one folder, one runner: no `cargo run`, no container to start by hand.

```sh
curl -fsSL https://giong.run/install.sh | sh
giong new api                 # a users API on SQLite that passes its tests
cd api
giong dev --background        # serve 127.0.0.1:8080, rebuild on save
curl http://127.0.0.1:8080/health
giong check                   # compiler, clippy, giong's checks
giong test                    # on io_uring and on epoll
giong dev stop                # the service; data/api.db kept
```

- [`giong new`](https://giong.run/docs/cli/new.md) writes the service, its tests, migrations, `.env.example`, a Dockerfile, CI and the files that teach coding agents.
- [`giong dev`](https://giong.run/docs/cli/dev.md) is the one way to run it while you work; `giong dev status` and `giong dev logs` say how it goes.
- [`giong check`](https://giong.run/docs/cli/check.md) is what to run before calling a change done; `giong fix` applies what needs no judgement.

Every command: https://giong.run/docs/cli.md

## Requirements

- **linux**: **Production.** io_uring on 6.0 or newer, 6.1+ recommended; developed and tested on 6.18. Elsewhere giong runs on its epoll driver, tested down to 5.15. Every crate.
- **macos**: **Development.** A kqueue driver, tested on macOS 15 and 26 on Apple Silicon. Every crate; correct, not tuned.
- **windows**: **Every crate.** I/O completion ports, tested on Windows Server 2022 and 2025: tasks, timers, channels, TCP, UDP, Unix sockets (Windows 10 1803 or newer), files, DNS, and the HTTP, TLS, database and other crates; correct, not tuned.
- **rust**: 1.80 or newer; 1.85 for giong-quic and giong-http's `unstable-http3`.
- **docker**: Docker's default seccomp profile blocks io_uring, so giong runs on epoll there by itself. [docs/DEPLOYMENT.md](https://giong.run/docs/deployment.md) shows how to allow io_uring in Docker, Compose and Kubernetes without turning seccomp off.
- **license**: MIT OR Apache-2.0, at your option.

## Install the crates

The crates are on crates.io and share one version number, 1.5.7.

```sh
cargo add giong
cargo add giong-web serde --features serde/derive   # routing, middleware, JSON
```

## A first server

`src/main.rs`:

```rust
use std::time::Duration;
use giong::net::TcpListener;
use giong_http::{Request, Response, Server};

async fn handler(req: Request) -> Response {
    match (req.method(), req.path()) {
        ("GET", "/health") => Response::text("ok"),
        ("POST", "/echo") => Response::new(200).body(req.body().to_vec()),
        _ => Response::new(404),
    }
}

fn main() -> std::io::Result<()> {
    giong::Builder::new()
        .shutdown_on_signals(true)                  // Ctrl-C / SIGTERM stop it gracefully
        .shutdown_grace(Duration::from_secs(10))    // requests in progress are answered
        .build()?
        .run(|_worker| async {
            let listener = TcpListener::bind("0.0.0.0:8080").expect("bind");
            Server::new().serve(listener, handler).await;
        })
}
```

```sh
cargo run --release          # listens on port 8080
curl localhost:8080/health   # ok
```

## What happens to a request

One connection stays on the worker that accepted it for its whole life. Every trip into the kernel that a turn of the worker's loop needs, for all of its connections, goes in the same `io_uring_enter`; a database query is one more entry in that ring.

## The documentation

Every page is also Markdown, for agents:

### Start

- [Best practices](https://giong.run/docs/best-practices.md): How to build a service on giong that is correct the first time, fast without tuning and easy to run: the shape of a service, state, blocking, timeouts, databases, deployment, tests.
- [Agent skill](https://giong.run/docs/agent-skill.md): A skill teaches a coding agent how to work with giong: the rules that are easy to get wrong, which crate does each job, how to port Tokio code, how to debug a running service. It comes with the README of every crate, the best practices, the deployment notes and the error codes of this release, so the agent reads the real documentation instead of guessing.

### Dev kit

- [Dev kit overview](https://giong.run/docs/cli.md): The giong command: start a project, run it with a restart on every save, check it, test it and ship it. One small binary; every command answers people in text and agents in JSON with --json.
- [giong new](https://giong.run/docs/cli/new.md): Start a project from a template (web, gRPC or worker) that builds, lints and passes its tests at once.
- [giong ai init](https://giong.run/docs/cli/ai.md): Teach the project to coding agents: AGENTS.md, CLAUDE.md, Cursor rules, the skill and .mcp.json.
- [giong dev](https://giong.run/docs/cli/dev.md): Build, run and restart on every save without refusing a request; one runner per project, also in the background.
- [giong check](https://giong.run/docs/cli/check.md): Compiler errors, clippy and giong's own checks (blocking calls, host names, borrows across .await) in one report.
- [giong fix](https://giong.run/docs/cli/fix.md): Apply the fixes that need no judgement, whole or not at all.
- [giong test](https://giong.run/docs/cli/test.md): Run the tests with .env, on io_uring and on epoll, with a time limit.
- [giong doctor](https://giong.run/docs/cli/doctor.md): Is this machine and this project ready: toolchain, kernel, io_uring, limits, ports, disk.
- [giong db](https://giong.run/docs/cli/db.md): A development PostgreSQL, MySQL or Redis in a container, and forward-only SQL migrations.
- [giong env](https://giong.run/docs/cli/env.md): Where each setting comes from, what is missing, secrets masked.
- [giong routes and giong openapi](https://giong.run/docs/cli/routes.md): The routes the running service serves, and an OpenAPI 3.1 document made from them.
- [giong record](https://giong.run/docs/cli/record.md): Turn real requests into test cases, with secrets kept out of the files, and replay them.
- [giong dashboard](https://giong.run/docs/cli/dashboard.md): One page on localhost: build state, live requests, routes, workers, pools and warnings.
- [giong top](https://giong.run/docs/cli/top.md): The running service seen from inside: workers, tasks, memory, pools, warnings.
- [giong bench](https://giong.run/docs/cli/bench.md): Load at a fixed rate without coordinated omission, and compare a change against a branch.
- [giong image](https://giong.run/docs/cli/image.md): An OCI image of the service without Docker or a Dockerfile.
- [giong clean](https://giong.run/docs/cli/clean.md): Free the disk old builds take, keeping what the next build needs.
- [giong mcp](https://giong.run/docs/cli/mcp.md): Every command as a tool for coding agents over MCP, read-only unless allowed.
- [giong upgrade](https://giong.run/docs/cli/upgrade.md): 
- [giong cache](https://giong.run/docs/cli/cache.md): 

### Guides

- [Build with a coding agent](https://giong.run/docs/guides/coding-agents.md): Set a project up for an agent once, then the loop it follows: one runner, checks in JSON, tools over MCP, and the docs as Markdown.
- [Safe defaults of the dev kit](https://giong.run/docs/guides/safe-defaults.md): What the giong command keeps safe by default: loopback only, a dashboard token, secrets out of recordings, read-only agents, development code out of production.
- [Use a database](https://giong.run/docs/guides/database.md): A development database in one command, forward-only migrations, and one pool per worker.
- [Test a service](https://giong.run/docs/guides/testing.md): Async unit tests, handler tests without a socket, recorded requests replayed, and giong test on both drivers.
- [Ship a service](https://giong.run/docs/guides/ship.md): Measure the change, build an image without Docker, and the few settings production needs.
- [Coming from Tokio](https://giong.run/docs/guides/from-tokio.md): The runtime model, Tokio's names in giong, and the giong crate for each job.

### Runtime

- [giong](https://giong.run/docs/giong.md): The runtime: TCP, UDP, Unix sockets, files, timers, DNS, channels and graceful shutdown.

### Protocols

- [giong-proto](https://giong.run/docs/giong-proto.md): Protocol Buffers decoded as views over the receive buffer. Strings, bytes, repeated fields and maps borrow from the input; encoding writes message structs whose fields borrow too.
- [giong-quic](https://giong.run/docs/giong-quic.md): QUIC (RFC 9000) on giong's UDP sockets, one endpoint per worker, with TLS 1.3 from rustls. It carries giong-http's HTTP/3 server and client.
- [giong-tls](https://giong.run/docs/giong-tls.md): TLS 1.2 and 1.3 for servers and clients on rustls with ring. No OpenSSL.
- [giong-http](https://giong.run/docs/giong-http.md): A small, strict HTTP/1.1 and HTTP/2 server with one async handler, and a client that speaks both.
- [giong-grpc](https://giong.run/docs/giong-grpc.md): gRPC servers and clients on giong-http's HTTP/2. Messages are read as giong-proto views without copies.
- [giong-compat](https://giong.run/docs/giong-compat.md): AsyncRead, AsyncWrite and AsyncBufRead from Tokio and futures-io for giong's streams, so protocol crates that take a stream run on giong with no Tokio runtime.

### Clients

- [giong-postgres](https://giong.run/docs/giong-postgres.md): A PostgreSQL client and pool. The statements of every task pipeline over a few connections.
- [giong-mysql](https://giong.run/docs/giong-mysql.md): A client and pool for MySQL 8 and MariaDB 10.6 or newer, built the same way as giong-postgres.
- [giong-redis](https://giong.run/docs/giong-redis.md): A Redis client and pool on RESP3. The commands of every task pipeline over one connection.

### Framework

- [giong-web](https://giong.run/docs/giong-web.md): A router, middleware and JSON on giong-http. Handlers are plain async fns; every error is a coded problem+json answer.

### Tooling

- [giong-grpc-build](https://giong.run/docs/giong-grpc-build.md): Generates giong-proto messages and, for each service, a trait, a server and a client from .proto files in build.rs. Parsed in Rust: no protoc to install.
- [giong-mcp](https://giong.run/docs/giong-mcp.md): A read-only MCP server that lets an AI agent look inside a running service: which task blocks a worker, which line spawned the tasks that leak, how full the pools are.

### Operate

- [Deployment](https://giong.run/docs/deployment.md): io_uring in Docker, Docker Compose and Kubernetes, the locked-memory limit, kernel quirks, restarting on the same port.
- [Stability](https://giong.run/docs/stability.md): What the 1.x promise covers: the API and its documented behaviour, error codes, environment variables, the minimum Rust version and the kernels.
- [Benchmark tables](https://giong.run/docs/benchmark-tables.md): Every benchmark with its method, machine and raw output, losses next to wins.
- [Error codes](https://giong.run/docs/errors.md): Every GIONG-E code: what went wrong and how to fix it.

## Examples

- [examples/rest-api](https://github.com/giong-run/Giong/tree/v1.5.7/examples/rest-api): A whole service on `giong-web`: users kept in PostgreSQL, read through a Redis cache, served over HTTPS, with request ids, a JSON access log, readiness, a streamed export and graceful shutdown. Its README has the routes, the configuration and how it is tested. `cargo run --release -p rest-api`
- [examples/echo.rs](https://github.com/giong-run/Giong/blob/v1.5.7/examples/echo.rs): A TCP echo server on every core. `cargo run --release --example echo`
- [examples/http.rs](https://github.com/giong-run/Giong/blob/v1.5.7/examples/http.rs): A small HTTP/1.1 server: keep-alive, pipelining, idle timeouts, a header size limit and graceful shutdown on SIGINT and SIGTERM. The same routes the benchmarks use. `cargo run --release --example http`
- [examples/udp_echo.rs](https://github.com/giong-run/Giong/blob/v1.5.7/examples/udp_echo.rs): A UDP echo server, receiving with multishot `recvmsg`. `cargo run --release --example udp_echo`
- [examples/cat.rs](https://github.com/giong-run/Giong/blob/v1.5.7/examples/cat.rs): Prints files to standard output, reading them through io_uring with positional reads. `cargo run --release --example cat -- Cargo.toml`
- [examples/stdio.rs](https://github.com/giong-run/Giong/blob/v1.5.7/examples/stdio.rs): A line protocol over standard input and output, as a local tool server speaks it: runs programs as child processes and answers with their output, and notes `SIGHUP`. `cargo run --release --example stdio`

## API reference

- [giong](https://docs.rs/giong/1.5.7): Executor, io_uring, epoll and kqueue drivers and IOCP; TCP, UDP, Unix sockets, files, timers, DNS, channels.
- [giong-proto](https://docs.rs/giong-proto/1.5.7): Protocol Buffers decoded as views over the receive buffer: no copy, no allocation per field.
- [giong-grpc-build](https://docs.rs/giong-grpc-build/1.5.7): Generates giong-proto messages and gRPC services from .proto files at build time.
- [giong-quic](https://docs.rs/giong-quic/1.5.7): QUIC on giong's own UDP sockets: endpoints, streams and 0-RTT, TLS 1.3 from rustls.
- [giong-tls](https://docs.rs/giong-tls/1.5.7): TLS 1.2 and 1.3 for servers and clients on rustls. No OpenSSL.
- [giong-http](https://docs.rs/giong-http/1.5.7): A strict HTTP/1.1 and HTTP/2 server, HTTPS behind a feature, and a pooled client.
- [giong-grpc](https://docs.rs/giong-grpc/1.5.7): gRPC servers and clients on giong-http's HTTP/2, messages read as giong-proto views.
- [giong-postgres](https://docs.rs/giong-postgres/1.5.7): PostgreSQL client and pool; statements of every task pipeline together.
- [giong-mysql](https://docs.rs/giong-mysql/1.5.7): MySQL and MariaDB client and pool, built the same way as giong-postgres.
- [giong-redis](https://docs.rs/giong-redis/1.5.7): Redis client on RESP3: pipelining across tasks, Pub/Sub, a pool.
- [giong-web](https://docs.rs/giong-web/1.5.7): Router, middleware and JSON on giong-http; errors as coded problem+json.
- [giong-mcp](https://docs.rs/giong-mcp/1.5.7): A read-only MCP server: an agent sees what each worker, task and pool is doing.
- [giong-compat](https://docs.rs/giong-compat/1.5.7): Tokio's and futures-io's I/O traits for giong's streams, for h2, tokio-rustls and the like.

## Error codes

Every error giong raises starts with a code, says what went wrong and how to fix it, and links to its page. Match on `giong::error::code(&err)`, never on the message.

```text
GIONG-E0102: no address found for db.internal. Fix: check the spelling of the name, /etc/hosts, and the DNS servers in /etc/resolv.conf (in a container, the network it is attached to). See https://github.com/giong-run/Giong/blob/v1.5.7/docs/errors/E0102.md
```

All 253 codes: https://giong.run/docs/errors.md

## When giong is the wrong choice

- **Tuned for Linux**: io_uring on 6.0 or newer, epoll elsewhere; on epoll giong is as fast as other epoll runtimes, not faster. macOS is for development, and so is Windows.
- **Not Tokio's ecosystem**: Crates that need the Tokio runtime (sqlx, reqwest, tonic's transport) do not run on giong. Crates generic over a Tokio or futures-io stream (h2, tokio-rustls) run through `giong-compat`, at the cost of a copy.
- **TLS certificates**: Certificates are not reloaded for you. Kernel TLS is off by default: it pays off for large files, not for data in memory.
- **Running tasks stay put**: New work can go to the least busy worker (`spawn_balanced`, `bind_balanced`), but a task never moves once it has started.
- **Some gaps**: Names come from `/etc/hosts` and DNS, not mDNS or LDAP. Also missing: Unix datagram sockets, multicast, file locks.

- **Monoio** is the mature choice and runs in production at ByteDance. If you need a runtime proven in production at scale today, use it.
- **Glommio** is built for storage engines: Direct I/O and priority scheduling are its strengths.
- **Tokio** is the default general-purpose runtime, with the largest ecosystem and years in production. giong aims to beat it on technical merit and replaces its crates with its own, but it has not run in production yet: measure your own workload before you switch.

## Questions people ask

**Is giong ready for production?**

It is at 1.x with a frozen API, tested hard on every change and in long soaks, and one service runs on it so far, its author's. Measure your own workload before you switch, and read the limits on this page.

**Does it run on macOS or Windows?**

macOS, yes, for development: a kqueue driver runs every crate, tested on macOS 15 and 26 on Apple Silicon, correct but not tuned. Windows, yes, every crate: I/O completion ports, tested on Windows Server 2022 and 2025, correct but not tuned; Linux is where giong is fast.

**Which Linux kernel do I need?**

For io_uring, 6.0 or newer, 6.1 or newer recommended. giong tries three ring setups in order: `DEFER_TASKRUN` (6.1+), `COOP_TASKRUN` (6.0), then no flags. On an older kernel, or where io_uring is blocked, it runs on its epoll driver (tested on 5.15).

**Can I use hyper, reqwest, sqlx or other Tokio crates?**

Not crates that need the Tokio runtime; giong has its own HTTP, TLS, PostgreSQL, MySQL and Redis crates instead. Crates that only use `std::future` work, and crates generic over a Tokio or futures-io stream (h2, tokio-rustls, tokio-tungstenite, hyper's connection API) run through `giong-compat`.

**Is it faster than Tokio?**

Sometimes, and the benchmarks page says where. Small responses at 50 connections are a tie. giong makes about 0.03 system calls per request against 2.05, spends the least CPU per request at 5,000 connections, and reads files 8 times faster than Tokio on one core. On bare metal with 8 workers it served 2.45M requests/s against raw Tokio's 2.16M.

**How do I report a security problem?**

Privately, through GitHub's private vulnerability reporting (the Security link at the bottom of every page), not in a public issue.

**Why the name?**

After Thánh Gióng, the child of Vietnamese legend who lay silent for three years and then rose a giant: next to nothing while idle, every core the moment load arrives.

## Work on giong

Read https://github.com/giong-run/Giong/blob/main/CONTRIBUTING.md first; coding agents have https://github.com/giong-run/Giong/blob/main/AGENTS.md.

```sh
git clone https://github.com/giong-run/Giong.git && cd Giong
cargo test --workspace --all-features   # the whole suite, every crate
```
