# giong clean

> giong 1.5.5 · https://giong.run/docs/cli/clean/ · source: https://github.com/giong-run/Giong/blob/v1.5.5/giong-cli/README.md#giong-clean

```sh
giong clean [dir...] [--days N] [--all] [--dry-run]
```

Frees the disk old builds use and keeps what the next build needs: removes what cargo built with another Rust version, and incremental data unused for 14 days, from this project's build directory, or from every cargo build directory under the folders given (`giong clean ~/code`). `--days N` also removes units unused for N days, `--all` empties the build directory, `--dry-run` only says what would go. It understands both of cargo's build-directory layouts and touches only directories cargo made.
