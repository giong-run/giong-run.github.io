# giong dashboard

> giong 1.5.5 · https://giong.run/docs/cli/dashboard/ · source: https://github.com/giong-run/Giong/blob/v1.5.5/giong-cli/README.md#giong-dashboard

```sh
giong dashboard [--port N] [--mcp ADDRESS] [--base-url URL] [--log FILE]
```

One page in the browser for the service `giong dev` runs in this project, on `http://127.0.0.1:7700`:

- the build state and the last compiler error
- the live request log, with status and time taken, and bodies when `Record` is on; each request can be copied as curl, sent again, or saved as a `giong record` case
- routes, with a Try button, curl lines and an OpenAPI download
- workers and tasks by the line that spawned them, with a one-second profile
- connection pools, the service's own output, and its warnings with their error pages
- a box to send any request to the service

It reads what the service already gives, its output and its giong-mcp, so the service runs no extra code for it. Only the person who started it can use it. It listens on 127.0.0.1 and answers only a loopback `Host`. Each start makes a random token, which is part of the address printed (`http://127.0.0.1:7700/#t=...`), and the API refuses any request without it (`GIONG-E5051`). The token comes after `#`, so no request line, log or `Referer` carries it, and the page takes it out of the address bar once read. A web page cannot read the dashboard or send requests through it, and neither can another program or user on the machine, though the request log holds bodies. `giong dev --ui` starts it with the service and prints its address.
