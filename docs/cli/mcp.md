# giong mcp

> giong 1.5.0 · https://giong.run/docs/cli/mcp/ · source: https://github.com/giong-run/Giong/blob/v1.5.0/giong-cli/README.md#giong-mcp

```sh
claude mcp add giong -- giong mcp
```

Serves every command as a tool to coding agents over the Model Context Protocol on standard input and output, each answering with the command's JSON document. `dev` is a tool for `status` and `logs` (read-only) and, with `--allow-writes`, `--background` and `stop`; `dev_status` reads `.giong/dev.json` as it is.
