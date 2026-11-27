# giong cache

> giong 1.5.5 · https://giong.run/docs/cli/cache/ · source: https://github.com/giong-run/Giong/blob/v1.5.5/giong-cli/README.md#giong-cache

```sh
giong cache [status]
giong cache share [dir...]
giong cache gc [--budget SIZE] [--dry-run]
```

Keeps one copy on disk of each dependency build for every project on the machine. Each Rust project builds its own copy of every dependency, and cargo never removes one; the build store replaces the files a registry or git dependency's build wrote with hard links to one copy in `~/.giong/store` whenever the bytes are the same. Each project keeps its own `target/`, so there is no shared lock to wait on, no binary of one project overwriting another's, and `cargo clean` in one project drops only that project's links. Cargo sees every build as fresh afterwards: the shared files get the same modification time in every project (2020-01-01 plus the unit's height in the dependency graph), so a unit stays newer than what it depends on. A build directory is changed only while giong holds cargo's own lock on it.

`giong dev` shares each build in the background once the service is ready, and runs `gc` once an hour. `giong cache share` does it for projects already built, giong or not (`giong cache share ~/code`). `giong cache gc` removes what no project links any more, and when the builds of the projects giong knows use more than the budget (`--budget`, `GIONG_STORE_BUDGET`, default `30G`, `off` for none) frees what an older Rust built and incremental data unused for 14 days, then the whole build of projects not built for 14 days, the longest idle first; never the project `giong dev` serves, never a build cargo is using (of any cargo version), and only directories cargo marked as builds (its `CACHEDIR.TAG`). `giong cache` says what the store saves. `GIONG_STORE=off` turns it off; `GIONG_STORE_DIR` moves it (it must be on the same drive as the projects). Needs cargo 1.100 or later (its build-dir layout) and a `giong` built with Rust 1.89 or later; anything else is left as it is (`GIONG-E5025`).
