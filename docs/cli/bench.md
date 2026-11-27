# giong bench

> giong 1.5.2 · https://giong.run/docs/cli/bench/ · source: https://github.com/giong-run/Giong/blob/v1.5.2/giong-cli/README.md#giong-bench

```sh
giong bench [<url>] [--rate <n>|auto|max] [--conns 50] [--duration 10]
giong bench --against main [--rounds 5] [--max-regression 5]
```

Load at a fixed rate, with each request's latency counted from when it was due, so a stall is charged for every request behind it (no coordinated omission): p50, p99, p99.9 and the maximum. `auto` finds the most the service answers and measures at half of it; `max` measures throughput. Without a URL it builds the service in release mode and starts it on a free port with every core. `--against <ref>` builds that commit too and alternates the two builds round by round; `--max-regression` fails only when the change is slower in every round of at least three. Every round goes to `.giong/bench/<time>.csv`. The load generator shares the machine with the service: numbers to publish come from two machines.
