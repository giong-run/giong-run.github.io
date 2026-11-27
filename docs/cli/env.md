# giong env

> giong 1.5.2 · https://giong.run/docs/cli/env/ · source: https://github.com/giong-run/Giong/blob/v1.5.2/giong-cli/README.md#giong-env

```sh
giong env [show|check|init|get <KEY>]
```

The service's configuration comes from the environment only; `.env` is for development. `show` lists every key of `.env.example`, `.env` and the environment with where its value comes from, secrets masked (`--reveal` shows them). `check` reports a key with no value at the line of `.env.example` that lists it, a key only in `.env`, and a line `.env` cannot read. `init` writes `.env` from `.env.example`; `get` prints one value for scripts.
