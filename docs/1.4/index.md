# giong 1.4 documentation

> v1.4.2 · https://giong.run/docs/1.4/

Every crate's README and the guides as they were released in v1.4.2, for code that depends on giong 1.4. The API reference is on docs.rs at the same version.

- [Best practices](https://giong.run/docs/1.4/best-practices.md): How to build a service on giong that is correct the first time, fast without tuning and easy to run: the shape of a service, state, blocking, timeouts, databases, deployment, tests.
- [giong](https://giong.run/docs/1.4/giong.md): The runtime: TCP, UDP, Unix sockets, files, timers, DNS, channels and graceful shutdown.
- [giong-proto](https://giong.run/docs/1.4/giong-proto.md): Protocol Buffers decoded as views over the receive buffer. Strings, bytes, repeated fields and maps borrow from the input; encoding writes message structs whose fields borrow too.
- [giong-grpc-build](https://giong.run/docs/1.4/giong-grpc-build.md): Generates giong-proto messages and, for each service, a trait, a server and a client from .proto files in build.rs. Parsed in Rust: no protoc to install.
- [giong-quic](https://giong.run/docs/1.4/giong-quic.md): QUIC (RFC 9000) on giong's UDP sockets, one endpoint per worker, with TLS 1.3 from rustls. It carries giong-http's HTTP/3 server and client.
- [giong-tls](https://giong.run/docs/1.4/giong-tls.md): TLS 1.2 and 1.3 for servers and clients on rustls with ring. No OpenSSL.
- [giong-http](https://giong.run/docs/1.4/giong-http.md): A small, strict HTTP/1.1 and HTTP/2 server with one async handler, and a client that speaks both.
- [giong-grpc](https://giong.run/docs/1.4/giong-grpc.md): gRPC servers and clients on giong-http's HTTP/2. Messages are read as giong-proto views without copies.
- [giong-postgres](https://giong.run/docs/1.4/giong-postgres.md): A PostgreSQL client and pool. The statements of every task pipeline over a few connections.
- [giong-mysql](https://giong.run/docs/1.4/giong-mysql.md): A client and pool for MySQL 8 and MariaDB 10.6 or newer, built the same way as giong-postgres.
- [giong-redis](https://giong.run/docs/1.4/giong-redis.md): A Redis client and pool on RESP3. The commands of every task pipeline over one connection.
- [giong-web](https://giong.run/docs/1.4/giong-web.md): A router, middleware and JSON on giong-http. Handlers are plain async fns; every error is a coded problem+json answer.
- [giong-mcp](https://giong.run/docs/1.4/giong-mcp.md): A read-only MCP server that lets an AI agent look inside a running service: which task blocks a worker, which line spawned the tasks that leak, how full the pools are.
- [giong-compat](https://giong.run/docs/1.4/giong-compat.md): AsyncRead, AsyncWrite and AsyncBufRead from Tokio and futures-io for giong's streams, so protocol crates that take a stream run on giong with no Tokio runtime.
- [Deployment](https://giong.run/docs/1.4/deployment.md): io_uring in Docker, Docker Compose and Kubernetes, the locked-memory limit, kernel quirks, restarting on the same port.
- [Stability](https://giong.run/docs/1.4/stability.md): What the 1.x promise covers: the API and its documented behaviour, error codes, environment variables, the minimum Rust version and the kernels.
- [Benchmark tables](https://giong.run/docs/1.4/benchmark-tables.md): Every benchmark with its method, machine and raw output, losses next to wins.
- [Error codes](https://giong.run/docs/1.4/errors.md): Every GIONG-E code: what went wrong and how to fix it.
