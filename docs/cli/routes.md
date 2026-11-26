# giong routes and giong openapi

> giong 1.5.1 · https://giong.run/docs/cli/routes/ · source: https://github.com/giong-run/Giong/blob/v1.5.1/giong-cli/README.md#giong-routes-and-giong-openapi

```sh
giong routes [--mcp ADDRESS] [--base-url URL]
giong openapi [--out FILE] [--title TEXT] [--mcp ADDRESS] [--base-url URL]
```

`giong routes` asks the running service which routes its giong-web apps serve: method, pattern, path parameters, streamed bodies, middleware, the line that added each route, and a curl line to try it. The service needs giong-mcp with its `web` feature; it is found through `.giong/dev.json` while `giong dev` runs, or `--mcp` (`GIONG_MCP` and `GIONG_MCP_TOKEN` otherwise).

`giong openapi` writes an OpenAPI 3.1 document from the same list: every path and method, path parameters, the coded problem+json answers giong-web makes itself (a parameter that does not parse, a body too long or not JSON, a handler that failed or timed out) and a curl sample per operation. Bodies are marked as not described, because giong-web reads them at run time and a guessed schema would mislead. Operation ids are unique, methods keep their case (only the standard upper-case ones are OpenAPI operations; others are listed under `x-giong-other-methods`), and routes of one shape share one path.
