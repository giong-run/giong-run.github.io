# giong record

> giong 1.5.6 · https://giong.run/docs/cli/record/ · source: https://github.com/giong-run/Giong/blob/v1.5.6/giong-cli/README.md#giong-record

```sh
giong record list [--route TEXT] [--method M] [--status 404|4xx] [--limit N] [--from LOG]
giong record show NUMBER
giong record save NUMBERS|3-7|last|all [--name NAME | --out FILE] [--append | --force] [--keep-secrets]
giong record replay [FILE...] [--base-url URL] [--timeout-ms N]
```

Turns real requests into tests. Add giong-web's `Record` middleware to the router; while `giong dev` runs, every answered request is written to `.giong/dev.log` with its headers and bodies, secret headers redacted. `list` and `show` read them, and `save` writes the chosen ones to `tests/recorded/<name>.json` as cases:

- dates, UUIDs, hex ids and tokens the service made become placeholders such as `{{uuid}}` and `{{time}}`
- an id the service made and a later request uses is captured (`{{capture:user_id}}`) and filled in again, so the cases pass against a service whose ids start elsewhere
- values the request itself sent stay literal, and the answer refers to them by name (`"no user {{user_id}}"`)
- secrets stay out of the file: redacted headers, and request fields whose names say secret (`password`, `token`, `api_key`, `client_secret`, `card_number`, `cvv` and the like) in the query, a form or a JSON body, are read from `GIONG_RECORD_<NAME>` when the cases run (`card.number` from `GIONG_RECORD_CARD_NUMBER`); such fields in answers become `{{string}}`
- `save` refuses, with `GIONG-E5050`, to write a value that still looks like a secret: a card number (by the Luhn check), a JWT, or a value under a secret name. `--keep-secrets` saves it anyway, for test values such as the `4242 4242 4242 4242` test card

`replay` sends the cases in order to the running service (or `--base-url`; from `.giong/dev.json` it takes only an address on this machine, so a cloned repository cannot send your environment's values elsewhere) and checks each answer: the status, `Content-Type`, the headers recorded and the body field by field. A difference is a `GIONG-E5066` error at the case's line with the expected and actual values, so `giong record replay` fails a CI run the way a test does. Case files are plain JSON, meant to be read, edited and committed.
