# Dev kit overview

> giong 1.5.4 · https://giong.run/docs/cli/ · source: https://github.com/giong-run/Giong/blob/v1.5.4/giong-cli/README.md

The `giong` command: start, run, check and ship services on the [Gióng](https://github.com/giong-run/Giong) async runtime. One small binary (the standard library, `serde_json`, and `socket2` on Unix), the same on Linux, macOS and Windows.

```sh
curl -fsSL https://giong.run/install.sh | sh                 # Linux, macOS
powershell -c "irm https://giong.run/install.ps1 | iex"     # Windows
cargo install giong-cli --locked                            # anywhere Rust runs
```

The install scripts download the prebuilt binary of the latest release from GitHub, check its SHA-256 and put it in `~/.giong/bin` (`%USERPROFILE%\.giong\bin` on Windows) without asking for root; where no binary is published they build it with cargo. `GIONG_VERSION=1.5.0` picks a version, `GIONG_INSTALL=/path` another folder, `GIONG_DOWNLOAD_URL` a mirror of the release downloads.

Each release archive, and the binary in it, has a signed [build provenance attestation](https://docs.github.com/en/actions/security-guides/using-artifact-attestations-to-establish-provenance-for-builds) saying which workflow built it from which commit, and a CycloneDX SBOM (`giong-<version>-<target>.cdx.json` on the Release) listing every crate compiled in. With the GitHub CLI, check an installed command or a downloaded archive:

```sh
gh attestation verify ~/.giong/bin/giong --repo giong-run/Giong \
  --signer-workflow giong-run/Giong/.github/workflows/release.yml
```

The binary also carries that list of crates itself ([cargo auditable](https://github.com/rust-secure-code/cargo-auditable), a few hundred bytes), so `cargo audit bin ~/.giong/bin/giong`, Trivy, Grype or osv-scanner can check it for known advisories. Then:

```sh
giong new api    # a users API on SQLite that builds and passes its tests now, nothing to install
giong doctor     # is this machine (and project) ready?
giong dev --background  # build, run on localhost and rebuild on every save; returns once it serves
giong dev status        # what runs, where, and how the last build went (logs, stop: the same way)
giong check      # compiler errors, clippy and giong's own checks in one report
giong upgrade    # move the project (and this command) to the newest giong release
```

## Commands

| Command | What it does |
|---|---|
| [`giong new`](https://giong.run/docs/cli/new.md) | Start a project from a template (web, gRPC or worker) that builds, lints and passes its tests at once. |
| [`giong ai init`](https://giong.run/docs/cli/ai.md) | Teach the project to coding agents: `AGENTS.md`, `CLAUDE.md`, Cursor rules, the skill and `.mcp.json`. |
| [`giong dev`](https://giong.run/docs/cli/dev.md) | Build, run and restart on every save without refusing a request; one runner per project, also in the background. |
| [`giong check`](https://giong.run/docs/cli/check.md) | Compiler errors, clippy and giong's own checks (blocking calls, host names, borrows across `.await`) in one report. |
| [`giong fix`](https://giong.run/docs/cli/fix.md) | Apply the fixes that need no judgement, whole or not at all. |
| [`giong test`](https://giong.run/docs/cli/test.md) | Run the tests with `.env`, on io_uring and on epoll, with a time limit. |
| [`giong doctor`](https://giong.run/docs/cli/doctor.md) | Is this machine and this project ready: toolchain, kernel, io_uring, limits, ports, disk. |
| [`giong db`](https://giong.run/docs/cli/db.md) | A development PostgreSQL, MySQL or Redis in a container, and forward-only SQL migrations. |
| [`giong env`](https://giong.run/docs/cli/env.md) | Where each setting comes from, what is missing, secrets masked. |
| [`giong routes and giong openapi`](https://giong.run/docs/cli/routes.md) | The routes the running service serves, and an OpenAPI 3.1 document made from them. |
| [`giong record`](https://giong.run/docs/cli/record.md) | Turn real requests into test cases, with secrets kept out of the files, and replay them. |
| [`giong dashboard`](https://giong.run/docs/cli/dashboard.md) | One page on localhost: build state, live requests, routes, workers, pools and warnings. |
| [`giong top`](https://giong.run/docs/cli/top.md) | The running service seen from inside: workers, tasks, memory, pools, warnings. |
| [`giong bench`](https://giong.run/docs/cli/bench.md) | Load at a fixed rate without coordinated omission, and compare a change against a branch. |
| [`giong image`](https://giong.run/docs/cli/image.md) | An OCI image of the service without Docker or a Dockerfile. |
| [`giong clean`](https://giong.run/docs/cli/clean.md) | Free the disk old builds take, keeping what the next build needs. |
| [`giong mcp`](https://giong.run/docs/cli/mcp.md) | Every command as a tool for coding agents over MCP, read-only unless allowed. |
| [`giong upgrade`](https://giong.run/docs/cli/upgrade.md) |  |

## One shape for every command

Every command prints text for people, or one JSON document with `--json` for agents and scripts. Every problem it reports is a finding with a code, a severity, the place in the source when there is one, the cause and the fix:

```json
{"command": "check", "version": "1.5.0", "ok": true,
 "findings": [{"code": "GIONG-E5006", "severity": "warning",
               "title": "std::fs blocks the worker inside an async function", "detail": "...", "fix": "...",
               "url": "https://github.com/giong-run/Giong/blob/v1.5.0/docs/errors/E5006.md",
               "file": "src/main.rs", "line": 14, "column": 19, "fixable": false}],
 "data": {"tool": "cargo clippy", "took_ms": 812}}
```

The exit status is 0 when nothing is wrong, 1 when a finding is an error, and 2 when the command could not do its job at all (no project here, cargo missing). `giong explain GIONG-E5006` prints the page of any giong error code, of the runtime, every giong crate or this command, without the network. Fields of the JSON document are only ever added, never renamed.

## Error codes

The command reports `GIONG-E5000` to `GIONG-E5099`; each has a page in [docs/errors](https://github.com/giong-run/Giong/blob/main/docs/errors/README.md) and in `giong explain`. Its findings can also carry the runtime's own codes, such as `GIONG-E0001` when io_uring is blocked.
