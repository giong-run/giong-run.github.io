# giong upgrade

> giong 1.5.1 · https://giong.run/docs/cli/upgrade/ · source: https://github.com/giong-run/Giong/blob/v1.5.1/giong-cli/README.md#giong-upgrade

```sh
giong upgrade [--to 1.5.0] [--dry-run] [--offline]
giong upgrade --self [--to 1.5.0] [--dry-run]
```

Moves every giong crate of the project to the newest release (or `--to`). The giong crates are released together with one version, and a 1.x release only adds, so this is one step: the version of each giong dependency in every `Cargo.toml` of the workspace, written as you wrote it (`"1.4"` becomes `"1.5"`, `"~1.4.2"` becomes `"~1.5.0"`; comments, features and every other byte stay), `cargo update` of the giong crates to exactly that release, the release's `rust-version` against rustc and the project's own `rust-version`, and `cargo check`. If a step fails, every file is put back as it was (`.giong/upgrade/` holds the originals while it runs). Then the agent files `giong ai init` wrote are refreshed for the new release. It never moves back, across a major version, or past a pinned requirement (`=1.4.2`: `GIONG-E5095`); path and git dependencies are left to you. `--dry-run` lists the edits; `--offline` moves to this command's own release with cargo offline.

This command writes templates and agent files for its own release, so a release newer than the command updates the command first, then the new command moves the project. `giong upgrade --self` updates only the command, the way it was installed: from the install script's folder with the release archive (checked against its SHA-256, `GIONG_DOWNLOAD_URL` honoured), or with `cargo install giong-cli --locked --version x.y.z`. A command installed another way is left alone, with the command to use (`GIONG-E5096`). In `giong mcp`, `upgrade --dry-run` reads and `upgrade` writes; neither ever replaces the command.
