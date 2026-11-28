# giong fix

> giong 1.5.7 · https://giong.run/docs/cli/fix/ · source: https://github.com/giong-run/Giong/blob/v1.5.7/giong-cli/README.md#giong-fix

```sh
giong fix [--dry-run] [--allow-dirty] [-- cargo arguments]
```

Applies the fixes that need no judgement: the compiler's and clippy's machine-applicable suggestions and the `edits` of `giong check`'s findings, then looks again, up to three rounds. Each fix is applied whole or not at all, two fixes that touch the same code are never both applied, an edit outside the project is refused, and a file that changed while it ran is not written (`GIONG-E5082`). It refuses a git repository with changes that are not committed (`GIONG-E5080`) unless `--allow-dirty`; `--dry-run` shows the diff. What it leaves needs a decision: `giong check` lists it.
