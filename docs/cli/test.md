# giong test

> giong 1.5.5 · https://giong.run/docs/cli/test/ · source: https://github.com/giong-run/Giong/blob/v1.5.5/giong-cli/README.md#giong-test

```sh
giong test [--quick] [--nextest] [--timeout SECS] [-- cargo test arguments]
```

Runs `cargo test` with the variables of `.env` (those not already set; cargo and rustc settings stay out, `GIONG-E5088`), then on Linux again with `GIONG_DRIVER=epoll`, the driver a container without io_uring gets. Each failing test is a `GIONG-E5083` finding naming the driver it failed on and what it printed. A run longer than `--timeout` (1800 s; 0 for none) is stopped with every process it started, naming the tests still running (`GIONG-E5086`). `--quick` runs once, `--nextest` uses cargo-nextest.
