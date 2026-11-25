# giong top

> giong 1.5.0 · https://giong.run/docs/cli/top/ · source: https://github.com/giong-run/Giong/blob/v1.5.0/giong-cli/README.md#giong-top

```sh
giong top [--interval 1000] [--once] [--profile <ms>] [--tasks 20]
```

The running service seen from inside, through its giong-mcp: workers (tasks, I/O in flight, polls), memory, threads and descriptors, the tasks by the line that spawned them, pools, and warnings, such as a worker stuck in one task (`GIONG-E5055`). Redraws every second; one snapshot with `--once`, `--json` or when the output is not a terminal. Nothing is recorded while nobody watches, and nothing needs a rebuild.
