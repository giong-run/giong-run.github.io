# giong check

> giong 1.5.6 · https://giong.run/docs/cli/check/ · source: https://github.com/giong-run/Giong/blob/v1.5.6/giong-cli/README.md#giong-check

```sh
giong check [--no-clippy] [-- cargo arguments]
```

Runs `cargo clippy` (or `cargo check`, when clippy is not installed or with `--no-clippy`) on every target of the workspace and adds giong's own checks of the source:

| Code | Finds |
|---|---|
| `GIONG-E5006` | a blocking call (`std::fs`, `std::thread::sleep`, `std::net::TcpStream::connect`, `std::process::Command`, `reqwest::blocking`, reading standard input) inside async code, where it stops every task of the worker |
| `GIONG-E5007` | a listener bound to a host name (`localhost:8080`), which binds whichever address the name resolves to first |
| `GIONG-E5008` | a `RefCell` borrow held across an `.await`, which panics when another task borrows it meanwhile |

Code in `spawn_blocking`, threads and tests is left out. Fixes that can be applied as they are come as `edits` in the JSON document. While `giong dev` runs, its last build is in the report too.
