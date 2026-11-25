# giong doctor

> giong 1.5.0 · https://giong.run/docs/cli/doctor/ · source: https://github.com/giong-run/Giong/blob/v1.5.0/giong-cli/README.md#giong-doctor

```sh
giong doctor [--machine]
```

Checks the Rust toolchain and the linker, the kernel and whether io_uring is allowed, the locked-memory and open-files limits, and a container's seccomp profile; inside a project also the crates that need the Tokio runtime, a dev profile that writes full debug information (slow builds), `.env` against `.env.example`, the port the service uses, and the disk its builds take. Every problem comes with its code and its fix.
