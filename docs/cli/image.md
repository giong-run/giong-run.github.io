# giong image

> giong 1.5.1 · https://giong.run/docs/cli/image/ · source: https://github.com/giong-run/Giong/blob/v1.5.1/giong-cli/README.md#giong-image

```sh
giong image [--tag name:tag] [--add static,migrations] [--load] [--push ghcr.io/owner/name:tag]
```

An OCI image of the service without Docker or a Dockerfile: one layer with the release binary at `/app`, CA certificates and a nonroot user, no shell; static with musl when that target is installed (`rustup target add x86_64-unknown-linux-musl`), else with this machine's glibc libraries. The same source gives the same digest. `--load` hands it to Docker or Podman, `--push` to skopeo, crane, Docker or Podman.
