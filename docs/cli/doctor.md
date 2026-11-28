# giong doctor

> giong 1.5.6 · https://giong.run/docs/cli/doctor/ · source: https://github.com/giong-run/Giong/blob/v1.5.6/giong-cli/README.md#giong-doctor

```sh
giong doctor [--machine] [--offline]
```

Checks the Rust toolchain and the linker, the kernel and whether io_uring is allowed, the locked-memory and open-files limits, and a container's seccomp profile; inside a project also the crates that need the Tokio runtime, a dev profile that writes full debug information (slow builds), `.env` against `.env.example`, the port the service uses, and the disk its builds take. Every problem comes with its code and its fix.

In a project that takes giong crates from crates.io, it also reads `Cargo.lock`: giong crates on different releases (`GIONG-E5091`), a newer giong release out (`GIONG-E5090`) and this command older than it (`GIONG-E5092`). The newest release comes from `cargo info giong`, with your cargo configuration, at most once a day (`~/.giong/cache/releases.json`); `--offline` or `CARGO_NET_OFFLINE=true` skips the question, and `GIONG_LATEST=1.5.0` answers it (a mirror, or a company that approves releases).
