# Error codes

> giong 1.5.5 · https://giong.run/docs/errors/ · source: https://github.com/giong-run/Giong/blob/v1.5.5/docs/errors/README.md

Every error giong raises itself starts with a code and ends with a link to its page here:

```text
GIONG-E0102: no address found for db.internal. Fix: check the spelling of the name,
/etc/hosts, and the DNS servers in /etc/resolv.conf (in a container, the network it is
attached to). See https://github.com/giong-run/Giong/blob/main/docs/errors/E0102.md
```

Errors giong only passes on, such as "connection refused" from the kernel, have no code.
Functions still return `std::io::Error`; match on the code, not on the message:

```rust
use giong::error::{self, Code};

fn on_error(err: &std::io::Error) {
    if error::code(err) == Some(Code::HostNotFound) {
        // ...
    }
}
```

giong-http, giong-web, giong-tls, giong-postgres, giong-mysql, giong-redis, giong-mcp and
giong-compat use the same codes. A code is never reused or renumbered, and its page stays
when it is no longer raised.

## The runtime

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E0001](https://giong.run/docs/errors/E0001.md) | `IoUringUnavailable` | io_uring is not available |
| [GIONG-E0002](https://giong.run/docs/errors/E0002.md) | `KernelTooOld` | the kernel is too old |
| [GIONG-E0003](https://giong.run/docs/errors/E0003.md) | `SignalsTaken` | another runtime handles the signals |
| [GIONG-E0004](https://giong.run/docs/errors/E0004.md) | `InvalidConfig` | invalid runtime setting |
| [GIONG-E0005](https://giong.run/docs/errors/E0005.md) | `Unexpected` | unexpected result from the kernel |
| [GIONG-E0006](https://giong.run/docs/errors/E0006.md) | `MixedReads` | read used after recv |
| [GIONG-E0007](https://giong.run/docs/errors/E0007.md) | `TimedOut` | the time limit passed |
| [GIONG-E0008](https://giong.run/docs/errors/E0008.md) | `NoSuchWorker` | no such worker |
| [GIONG-E0009](https://giong.run/docs/errors/E0009.md) | `BufferRingRefused` | the kernel refused io_uring buffer rings |
| [GIONG-E0010](https://giong.run/docs/errors/E0010.md) | `LockedMemoryLimit` | the locked-memory limit is used up |
| [GIONG-E0011](https://giong.run/docs/errors/E0011.md) | `NotInRuntime` | not inside a giong runtime |
| [GIONG-E0012](https://giong.run/docs/errors/E0012.md) | `RuntimeAlreadyRunning` | a runtime already runs on this thread |
| [GIONG-E0013](https://giong.run/docs/errors/E0013.md) | `WrongWorker` | an operation was polled on another worker |
| [GIONG-E0014](https://giong.run/docs/errors/E0014.md) | `ComputeBusy` | the compute lane is full |
| [GIONG-E0016](https://giong.run/docs/errors/E0016.md) | `SignalNotAllowed` | this signal cannot be listened for |
| [GIONG-E0017](https://giong.run/docs/errors/E0017.md) | `FdPassing` | descriptors could not be passed |
| [GIONG-E0018](https://giong.run/docs/errors/E0018.md) | `ActivatedSocket` | the inherited socket cannot be used that way |
| [GIONG-E0019](https://giong.run/docs/errors/E0019.md) | `ChildProcess` | cannot watch the child process |
| [GIONG-E0020](https://giong.run/docs/errors/E0020.md) | `ThreadLimit` | a worker thread could not be started |
| [GIONG-E0080](https://giong.run/docs/errors/E0080.md) | `BudgetFull` | the budget is at its limit |
| [GIONG-E0081](https://giong.run/docs/errors/E0081.md) | `BudgetTooSmall` | a reservation larger than the budget |

## macOS

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E0028](https://giong.run/docs/errors/E0028.md) | `NetworkBuffersExhausted` | the system ran out of network buffers |

## Windows

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E0060](https://giong.run/docs/errors/E0060.md) | `UnixSocketInUse` | a file is already at the Unix socket path |
| [GIONG-E0061](https://giong.run/docs/errors/E0061.md) | `UnixPathTooLong` | Unix socket path too long |
| [GIONG-E0062](https://giong.run/docs/errors/E0062.md) | `UnixSocketsUnavailable` | no Unix domain sockets on this Windows |

## Messages between processes (`giong::ipc`)

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E0090](https://giong.run/docs/errors/E0090.md) | `IpcMessageTooLarge` | IPC message too large |
| [GIONG-E0091](https://giong.run/docs/errors/E0091.md) | `IpcCorrupted` | IPC channel memory corrupted |
| [GIONG-E0092](https://giong.run/docs/errors/E0092.md) | `IpcCapacity` | IPC capacity out of range |
| [GIONG-E0093](https://giong.run/docs/errors/E0093.md) | `IpcHandshake` | IPC handshake failed |
| [GIONG-E0094](https://giong.run/docs/errors/E0094.md) | `IpcPeerGone` | IPC peer process gone |
| [GIONG-E0095](https://giong.run/docs/errors/E0095.md) | `IpcReceiverClosed` | IPC receiver closed |

## Network and name resolution

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E0100](https://giong.run/docs/errors/E0100.md) | `InvalidAddress` | invalid address |
| [GIONG-E0101](https://giong.run/docs/errors/E0101.md) | `InvalidHostName` | invalid host name |
| [GIONG-E0102](https://giong.run/docs/errors/E0102.md) | `HostNotFound` | host not found |
| [GIONG-E0103](https://giong.run/docs/errors/E0103.md) | `NoAddresses` | no address to use |
| [GIONG-E0104](https://giong.run/docs/errors/E0104.md) | `NoDnsServer` | no DNS server configured |
| [GIONG-E0105](https://giong.run/docs/errors/E0105.md) | `DnsTimeout` | DNS server did not answer |
| [GIONG-E0106](https://giong.run/docs/errors/E0106.md) | `DnsServerFailure` | DNS server failed |
| [GIONG-E0107](https://giong.run/docs/errors/E0107.md) | `DnsBadResponse` | malformed DNS answer |
| [GIONG-E0108](https://giong.run/docs/errors/E0108.md) | `DnsServerUnreachable` | cannot reach the DNS server |
| [GIONG-E0109](https://giong.run/docs/errors/E0109.md) | `EmptyBuffer` | buffer with no room |
| [GIONG-E0110](https://giong.run/docs/errors/E0110.md) | `RecvBufferTooSmall` | receive buffers too small for GRO |
| [GIONG-E0111](https://giong.run/docs/errors/E0111.md) | `RecvAlreadyStarted` | receive option set after recv started |

## Files

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E0200](https://giong.run/docs/errors/E0200.md) | `InvalidPath` | invalid path |
| [GIONG-E0201](https://giong.run/docs/errors/E0201.md) | `FileTooShort` | the file ended before the bytes to send |
| [GIONG-E0202](https://giong.run/docs/errors/E0202.md) | `OffsetOutOfRange` | file offset out of range |
| [GIONG-E0203](https://giong.run/docs/errors/E0203.md) | `InvalidOpenOptions` | invalid combination of open options |
| [GIONG-E0204](https://giong.run/docs/errors/E0204.md) | `FileTooLarge` | file too large to read into memory |
| [GIONG-E0205](https://giong.run/docs/errors/E0205.md) | `OutsideRoot` | the path leads outside the root |

## giong-http

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E1101](https://giong.run/docs/errors/E1101.md) | `HttpClientRequest` | invalid HTTP request |
| [GIONG-E1102](https://giong.run/docs/errors/E1102.md) | `HttpClientConnect` | cannot connect to the HTTP server |
| [GIONG-E1103](https://giong.run/docs/errors/E1103.md) | `HttpClientTimeout` | HTTP request timed out |
| [GIONG-E1104](https://giong.run/docs/errors/E1104.md) | `HttpClientTooLarge` | HTTP response too large |
| [GIONG-E1105](https://giong.run/docs/errors/E1105.md) | `HttpClientRedirect` | redirect not followed |
| [GIONG-E1106](https://giong.run/docs/errors/E1106.md) | `HttpClientClosed` | connection closed before the response |
| [GIONG-E1107](https://giong.run/docs/errors/E1107.md) | `HttpClientProtocol` | malformed HTTP response |
| [GIONG-E1108](https://giong.run/docs/errors/E1108.md) | `HttpClientStatus` | HTTP error status |
| [GIONG-E1109](https://giong.run/docs/errors/E1109.md) | `HttpClientDecode` | cannot decode the response body |
| [GIONG-E1110](https://giong.run/docs/errors/E1110.md) | `HttpClientProxy` | the HTTP proxy refused the request |
| [GIONG-E1111](https://giong.run/docs/errors/E1111.md) | `HttpBodyTooLarge` | request body too large |
| [GIONG-E1112](https://giong.run/docs/errors/E1112.md) | `HttpBodyIncomplete` | request body cut short |
| [GIONG-E1113](https://giong.run/docs/errors/E1113.md) | `HttpBodyMalformed` | malformed request body |
| [GIONG-E1114](https://giong.run/docs/errors/E1114.md) | `HttpBodyNotSent` | response body not sent |
| [GIONG-E1115](https://giong.run/docs/errors/E1115.md) | `Http2Protocol` | HTTP/2 protocol error |
| [GIONG-E1116](https://giong.run/docs/errors/E1116.md) | `Http2TooManyResets` | too many HTTP/2 streams reset |
| [GIONG-E1117](https://giong.run/docs/errors/E1117.md) | `Http2Flood` | too many HTTP/2 control frames |
| [GIONG-E1118](https://giong.run/docs/errors/E1118.md) | `Http2HeadersTooLarge` | HTTP/2 header block too large |
| [GIONG-E1119](https://giong.run/docs/errors/E1119.md) | `HttpInvalidResponse` | invalid HTTP response |
| [GIONG-E1120](https://giong.run/docs/errors/E1120.md) | `HttpServerBusy` | request refused: the server's memory budget for requests is full |
| [GIONG-E1121](https://giong.run/docs/errors/E1121.md) | `HttpServerLowMemory` | connection refused: the server is low on memory |
| [GIONG-E1122](https://giong.run/docs/errors/E1122.md) | `HttpInvalidTrailer` | invalid HTTP trailer field |
| [GIONG-E1123](https://giong.run/docs/errors/E1123.md) | `HttpServerOutOfFiles` | connection not accepted: the server is out of file descriptors |
| [GIONG-E1124](https://giong.run/docs/errors/E1124.md) | `HttpServerAddressLimit` | connection refused: the client's address has too many connections open |

## giong-web

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E1201](https://giong.run/docs/errors/E1201.md) | `WebInvalidRoute` | invalid route pattern |
| [GIONG-E1202](https://giong.run/docs/errors/E1202.md) | `WebDuplicateRoute` | route added twice |
| [GIONG-E1203](https://giong.run/docs/errors/E1203.md) | `WebNoRoute` | no route for the path |
| [GIONG-E1204](https://giong.run/docs/errors/E1204.md) | `WebMethodNotAllowed` | method not allowed |
| [GIONG-E1205](https://giong.run/docs/errors/E1205.md) | `WebBadParameter` | invalid path or query parameter |
| [GIONG-E1206](https://giong.run/docs/errors/E1206.md) | `WebNoSuchParameter` | the route has no such parameter |
| [GIONG-E1207](https://giong.run/docs/errors/E1207.md) | `WebBadBody` | unreadable request body |
| [GIONG-E1208](https://giong.run/docs/errors/E1208.md) | `WebUnsupportedMediaType` | unsupported Content-Type |
| [GIONG-E1209](https://giong.run/docs/errors/E1209.md) | `WebTimeout` | no response in time |
| [GIONG-E1210](https://giong.run/docs/errors/E1210.md) | `WebInternal` | the handler failed |
| [GIONG-E1211](https://giong.run/docs/errors/E1211.md) | `WebUnavailable` | a service the handler needs is unavailable |
| [GIONG-E1212](https://giong.run/docs/errors/E1212.md) | `WebCorsRejected` | cross-origin request refused |
| [GIONG-E1213](https://giong.run/docs/errors/E1213.md) | `WebConfig` | invalid giong-web setting |

## giong-mcp

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E1301](https://giong.run/docs/errors/E1301.md) | `McpNotLocal` | the MCP server listens on this machine only |
| [GIONG-E1302](https://giong.run/docs/errors/E1302.md) | `McpBadAddress` | invalid address for the MCP server |
| [GIONG-E1303](https://giong.run/docs/errors/E1303.md) | `McpBind` | the MCP server cannot listen on its address |
| [GIONG-E1304](https://giong.run/docs/errors/E1304.md) | `McpNoToken` | the MCP server needs a token on TCP |
| [GIONG-E1305](https://giong.run/docs/errors/E1305.md) | `McpBadToken` | invalid token for the MCP server |

## giong-compat

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E1401](https://giong.run/docs/errors/E1401.md) | `CompatWriteFailed` | an earlier buffered write failed |
| [GIONG-E1402](https://giong.run/docs/errors/E1402.md) | `CompatShutDown` | write after shutdown |
| [GIONG-E1403](https://giong.run/docs/errors/E1403.md) | `CompatConfig` | invalid giong-compat setting |

## giong-tls

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E2001](https://giong.run/docs/errors/E2001.md) | `NoTrustedRoots` | no trusted certificate authorities |
| [GIONG-E2002](https://giong.run/docs/errors/E2002.md) | `BadCertificateData` | unreadable certificate data |
| [GIONG-E2003](https://giong.run/docs/errors/E2003.md) | `InvalidServerName` | invalid TLS server name |
| [GIONG-E2004](https://giong.run/docs/errors/E2004.md) | `TlsCertificate` | certificate rejected |
| [GIONG-E2005](https://giong.run/docs/errors/E2005.md) | `TlsProtocol` | TLS protocol error |
| [GIONG-E2006](https://giong.run/docs/errors/E2006.md) | `TlsHandshakeTimeout` | TLS handshake timed out |
| [GIONG-E2007](https://giong.run/docs/errors/E2007.md) | `TlsHandshakeEof` | connection closed during the TLS handshake |
| [GIONG-E2008](https://giong.run/docs/errors/E2008.md) | `TlsBroken` | TLS stream broken |
| [GIONG-E2009](https://giong.run/docs/errors/E2009.md) | `KtlsUnavailable` | kernel TLS is not available |
| [GIONG-E2010](https://giong.run/docs/errors/E2010.md) | `KtlsSetup` | the kernel refused the TLS keys |
| [GIONG-E2011](https://giong.run/docs/errors/E2011.md) | `KtlsKeyUpdate` | the kernel cannot change TLS keys |
| [GIONG-E2012](https://giong.run/docs/errors/E2012.md) | `TlsKeysExhausted` | TLS keys used up |
| [GIONG-E2013](https://giong.run/docs/errors/E2013.md) | `TlsConfig` | TLS setting not possible here |

## giong-postgres

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E3001](https://giong.run/docs/errors/E3001.md) | `PostgresConfig` | invalid PostgreSQL configuration |
| [GIONG-E3002](https://giong.run/docs/errors/E3002.md) | `PostgresConnect` | cannot connect to PostgreSQL |
| [GIONG-E3003](https://giong.run/docs/errors/E3003.md) | `PostgresAuth` | PostgreSQL authentication failed |
| [GIONG-E3004](https://giong.run/docs/errors/E3004.md) | `PostgresServer` | PostgreSQL server error |
| [GIONG-E3005](https://giong.run/docs/errors/E3005.md) | `PostgresType` | PostgreSQL type mismatch |
| [GIONG-E3006](https://giong.run/docs/errors/E3006.md) | `PostgresRowCount` | wrong number of rows |
| [GIONG-E3007](https://giong.run/docs/errors/E3007.md) | `PostgresTooLarge` | PostgreSQL result too large |
| [GIONG-E3008](https://giong.run/docs/errors/E3008.md) | `PostgresClosed` | PostgreSQL connection closed |
| [GIONG-E3009](https://giong.run/docs/errors/E3009.md) | `PostgresPoolTimeout` | PostgreSQL pool wait timed out |
| [GIONG-E3010](https://giong.run/docs/errors/E3010.md) | `PostgresProtocol` | PostgreSQL protocol error |
| [GIONG-E3011](https://giong.run/docs/errors/E3011.md) | `PostgresStatement` | statement refused before sending |

## giong-mysql

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E3101](https://giong.run/docs/errors/E3101.md) | `MysqlConfig` | invalid MySQL configuration |
| [GIONG-E3102](https://giong.run/docs/errors/E3102.md) | `MysqlConnect` | cannot connect to MySQL |
| [GIONG-E3103](https://giong.run/docs/errors/E3103.md) | `MysqlAuth` | MySQL authentication failed |
| [GIONG-E3104](https://giong.run/docs/errors/E3104.md) | `MysqlServer` | MySQL server error |
| [GIONG-E3105](https://giong.run/docs/errors/E3105.md) | `MysqlType` | MySQL type mismatch |
| [GIONG-E3106](https://giong.run/docs/errors/E3106.md) | `MysqlRowCount` | wrong number of rows |
| [GIONG-E3107](https://giong.run/docs/errors/E3107.md) | `MysqlTooLarge` | MySQL result too large |
| [GIONG-E3108](https://giong.run/docs/errors/E3108.md) | `MysqlClosed` | MySQL connection closed |
| [GIONG-E3109](https://giong.run/docs/errors/E3109.md) | `MysqlPoolTimeout` | MySQL pool wait timed out |
| [GIONG-E3110](https://giong.run/docs/errors/E3110.md) | `MysqlProtocol` | MySQL protocol error |
| [GIONG-E3111](https://giong.run/docs/errors/E3111.md) | `MysqlStatement` | statement refused before sending |

## giong-redis

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E3201](https://giong.run/docs/errors/E3201.md) | `RedisConfig` | invalid Redis configuration |
| [GIONG-E3202](https://giong.run/docs/errors/E3202.md) | `RedisConnect` | cannot connect to Redis |
| [GIONG-E3203](https://giong.run/docs/errors/E3203.md) | `RedisAuth` | Redis authentication failed |
| [GIONG-E3204](https://giong.run/docs/errors/E3204.md) | `RedisServer` | Redis server error |
| [GIONG-E3205](https://giong.run/docs/errors/E3205.md) | `RedisType` | Redis type mismatch |
| [GIONG-E3206](https://giong.run/docs/errors/E3206.md) | `RedisCommand` | command refused before sending |
| [GIONG-E3207](https://giong.run/docs/errors/E3207.md) | `RedisTooLarge` | Redis reply too large |
| [GIONG-E3208](https://giong.run/docs/errors/E3208.md) | `RedisClosed` | Redis connection closed |
| [GIONG-E3209](https://giong.run/docs/errors/E3209.md) | `RedisPoolTimeout` | Redis pool wait timed out |
| [GIONG-E3210](https://giong.run/docs/errors/E3210.md) | `RedisProtocol` | Redis protocol error |
| [GIONG-E3211](https://giong.run/docs/errors/E3211.md) | `RedisTimeout` | Redis reply timed out |
| [GIONG-E3601](https://giong.run/docs/errors/E3601.md) | `QuicConfig` | invalid QUIC configuration |
| [GIONG-E3602](https://giong.run/docs/errors/E3602.md) | `QuicConnect` | cannot set up the QUIC connection |
| [GIONG-E3603](https://giong.run/docs/errors/E3603.md) | `QuicConnectionLost` | the QUIC connection was lost |
| [GIONG-E3604](https://giong.run/docs/errors/E3604.md) | `QuicStreamReset` | the QUIC stream was reset |
| [GIONG-E3605](https://giong.run/docs/errors/E3605.md) | `QuicEarlyDataRejected` | the server refused the 0-RTT data |

## giong-proto

A prototype for giong 1.1, not published yet; its codes are `giong_proto::ErrorKind` until it joins `giong::error::Code`.

| Code | `ErrorKind::` | Meaning |
|---|---|---|
| [GIONG-E4001](https://giong.run/docs/errors/E4001.md) | `ErrorKind::Truncated` | the message ends in the middle of a field |
| [GIONG-E4002](https://giong.run/docs/errors/E4002.md) | `ErrorKind::BadVarint` | malformed varint |
| [GIONG-E4003](https://giong.run/docs/errors/E4003.md) | `ErrorKind::BadTag` | invalid field tag |
| [GIONG-E4004](https://giong.run/docs/errors/E4004.md) | `ErrorKind::BadGroup` | unbalanced group |
| [GIONG-E4005](https://giong.run/docs/errors/E4005.md) | `ErrorKind::BadUtf8` | string field is not UTF-8 |
| [GIONG-E4006](https://giong.run/docs/errors/E4006.md) | `ErrorKind::TooDeep` | messages nested too deeply |
| [GIONG-E4007](https://giong.run/docs/errors/E4007.md) | `ErrorKind::BadPacked` | malformed packed field |
| [GIONG-E4008](https://giong.run/docs/errors/E4008.md) | `ErrorKind::TooLarge` | message larger than 2 GiB |

## giong-grpc

New in giong 1.1. A call that fails ends with a gRPC status; the statuses giong-grpc raises itself carry one of these codes at the start of their message and in `Status::error_code`.

| Code | `Code::` | Meaning |
|---|---|---|
| [GIONG-E4101](https://giong.run/docs/errors/E4101.md) | `GrpcInvalidMetadata` | invalid gRPC metadata |
| [GIONG-E4102](https://giong.run/docs/errors/E4102.md) | `GrpcMessageTooLarge` | gRPC message too large |
| [GIONG-E4103](https://giong.run/docs/errors/E4103.md) | `GrpcProtocol` | gRPC protocol error |
| [GIONG-E4104](https://giong.run/docs/errors/E4104.md) | `GrpcEncoding` | unsupported gRPC message encoding |
| [GIONG-E4105](https://giong.run/docs/errors/E4105.md) | `GrpcDecode` | gRPC message is not valid protobuf |
| [GIONG-E4106](https://giong.run/docs/errors/E4106.md) | `GrpcTransport` | gRPC connection failed |
| [GIONG-E4107](https://giong.run/docs/errors/E4107.md) | `GrpcEndpoint` | invalid gRPC endpoint |
| [GIONG-E4108](https://giong.run/docs/errors/E4108.md) | `GrpcDeadline` | gRPC deadline exceeded |
| [GIONG-E4109](https://giong.run/docs/errors/E4109.md) | `GrpcCancelled` | gRPC call cancelled |
| [GIONG-E4110](https://giong.run/docs/errors/E4110.md) | `GrpcUnimplemented` | gRPC method not found |

## giong-cli

The `giong` command reports `GIONG-E5000` to `GIONG-E5099`. Its findings can also carry the runtime's own codes (`GIONG-E0001` when io_uring is blocked, for example).

| Code | Name | Meaning |
|---|---|---|
| [GIONG-E5000](https://giong.run/docs/errors/E5000.md) | `Usage` | the command line is not valid |
| [GIONG-E5001](https://giong.run/docs/errors/E5001.md) | `NotAProject` | no Cargo project here |
| [GIONG-E5002](https://giong.run/docs/errors/E5002.md) | `CargoFailed` | cargo could not be run |
| [GIONG-E5003](https://giong.run/docs/errors/E5003.md) | `RustTooOld` | the Rust compiler is too old |
| [GIONG-E5004](https://giong.run/docs/errors/E5004.md) | `TokioCrate` | a dependency needs the Tokio runtime |
| [GIONG-E5005](https://giong.run/docs/errors/E5005.md) | `UnknownCode` | no such error code |
| [GIONG-E5006](https://giong.run/docs/errors/E5006.md) | `BlockingCall` | a blocking call inside a worker |
| [GIONG-E5007](https://giong.run/docs/errors/E5007.md) | `HostNameBind` | binding a host name |
| [GIONG-E5008](https://giong.run/docs/errors/E5008.md) | `BorrowAcrossAwait` | a RefCell borrow held across an await |
| [GIONG-E5009](https://giong.run/docs/errors/E5009.md) | `BuildFailed` | the code does not compile |
| [GIONG-E5010](https://giong.run/docs/errors/E5010.md) | `Lint` | clippy has a finding |
| [GIONG-E5011](https://giong.run/docs/errors/E5011.md) | `DiskUsed` | build directories use a lot of disk |
| [GIONG-E5012](https://giong.run/docs/errors/E5012.md) | `SlowDebugInfo` | the dev profile writes full debug information |
| [GIONG-E5013](https://giong.run/docs/errors/E5013.md) | `OpenFilesLimit` | the open-files limit is low |
| [GIONG-E5014](https://giong.run/docs/errors/E5014.md) | `PortInUse` | the port is in use |
| [GIONG-E5015](https://giong.run/docs/errors/E5015.md) | `DockerSeccomp` | Docker blocks io_uring |
| [GIONG-E5016](https://giong.run/docs/errors/E5016.md) | `EnvMissing` | a variable from .env.example is not set |
| [GIONG-E5017](https://giong.run/docs/errors/E5017.md) | `ServiceExited` | the service stopped |
| [GIONG-E5018](https://giong.run/docs/errors/E5018.md) | `ServiceNotReady` | the service did not start listening |
| [GIONG-E5019](https://giong.run/docs/errors/E5019.md) | `McpUnreachable` | the service's giong-mcp does not answer |
| [GIONG-E5020](https://giong.run/docs/errors/E5020.md) | `WatchLimit` | too many files to watch |
| [GIONG-E5021](https://giong.run/docs/errors/E5021.md) | `NoLinker` | no linker found |
| [GIONG-E5022](https://giong.run/docs/errors/E5022.md) | `DevRunning` | giong dev already serves this project |
| [GIONG-E5023](https://giong.run/docs/errors/E5023.md) | `CompilerWarning` | the compiler has a warning |
| [GIONG-E5024](https://giong.run/docs/errors/E5024.md) | `ListensOnNetwork` | the service would listen on every network interface |
| [GIONG-E5025](https://giong.run/docs/errors/E5025.md) | `StoreUnusable` | the build store cannot share a build |
| [GIONG-E5030](https://giong.run/docs/errors/E5030.md) | `BadProjectName` | the project name is not valid |
| [GIONG-E5031](https://giong.run/docs/errors/E5031.md) | `TargetNotEmpty` | the directory for the new project is not empty |
| [GIONG-E5032](https://giong.run/docs/errors/E5032.md) | `BadTemplateChoice` | the template does not offer that choice |
| [GIONG-E5033](https://giong.run/docs/errors/E5033.md) | `WriteFailed` | a file of the project could not be written |
| [GIONG-E5034](https://giong.run/docs/errors/E5034.md) | `WindowsLimited` | the project's crates do not build on Windows |
| [GIONG-E5035](https://giong.run/docs/errors/E5035.md) | `BadGiongPath` | --giong-path is not a giong checkout |
| [GIONG-E5036](https://giong.run/docs/errors/E5036.md) | `AgentFileForeign` | an agent file exists that giong did not write |
| [GIONG-E5037](https://giong.run/docs/errors/E5037.md) | `McpJsonInvalid` | .mcp.json is not valid JSON |
| [GIONG-E5038](https://giong.run/docs/errors/E5038.md) | `VersionMismatch` | the project uses another giong version than this command |
| [GIONG-E5039](https://giong.run/docs/errors/E5039.md) | `NotGiongProject` | the project does not depend on giong |
| [GIONG-E5040](https://giong.run/docs/errors/E5040.md) | `NoContainerRuntime` | neither Docker nor Podman answers |
| [GIONG-E5041](https://giong.run/docs/errors/E5041.md) | `DbNotReady` | the database does not answer queries |
| [GIONG-E5042](https://giong.run/docs/errors/E5042.md) | `DbUrl` | no usable database URL |
| [GIONG-E5043](https://giong.run/docs/errors/E5043.md) | `MigrationChanged` | an applied migration was changed |
| [GIONG-E5044](https://giong.run/docs/errors/E5044.md) | `MigrationFailed` | a migration failed |
| [GIONG-E5045](https://giong.run/docs/errors/E5045.md) | `MigrationName` | a migration file is not valid |
| [GIONG-E5046](https://giong.run/docs/errors/E5046.md) | `DbClientMissing` | no database client to run the SQL |
| [GIONG-E5047](https://giong.run/docs/errors/E5047.md) | `MigrationUnknown` | the database has a migration the project does not |
| [GIONG-E5048](https://giong.run/docs/errors/E5048.md) | `DbNotManaged` | the database was not started by giong db up |
| [GIONG-E5049](https://giong.run/docs/errors/E5049.md) | `DbStartFailed` | the database container could not start |
| [GIONG-E5050](https://giong.run/docs/errors/E5050.md) | `RecordSecret` | a recorded value looks like a secret |
| [GIONG-E5051](https://giong.run/docs/errors/E5051.md) | `DashboardToken` | the dashboard refused a request without its token |
| [GIONG-E5053](https://giong.run/docs/errors/E5053.md) | `DashboardPortInUse` | the dashboard's port is in use |
| [GIONG-E5054](https://giong.run/docs/errors/E5054.md) | `DashboardFailed` | the dashboard could not do what the page asked |
| [GIONG-E5055](https://giong.run/docs/errors/E5055.md) | `WorkerBlocked` | a worker of the service is stuck in one task |
| [GIONG-E5056](https://giong.run/docs/errors/E5056.md) | `ServiceWarning` | the service reports a warning |
| [GIONG-E5057](https://giong.run/docs/errors/E5057.md) | `NoService` | no running service to ask |
| [GIONG-E5058](https://giong.run/docs/errors/E5058.md) | `McpNoTool` | the service's giong-mcp lacks a tool |
| [GIONG-E5059](https://giong.run/docs/errors/E5059.md) | `McpToolError` | the service's giong-mcp refused a call |
| [GIONG-E5060](https://giong.run/docs/errors/E5060.md) | `BenchTarget` | the service to measure cannot be reached |
| [GIONG-E5061](https://giong.run/docs/errors/E5061.md) | `BenchAgainst` | the build to compare against cannot be made |
| [GIONG-E5062](https://giong.run/docs/errors/E5062.md) | `BenchGeneratorBehind` | the load generator fell behind its schedule |
| [GIONG-E5063](https://giong.run/docs/errors/E5063.md) | `BenchErrors` | requests failed during the benchmark |
| [GIONG-E5064](https://giong.run/docs/errors/E5064.md) | `BenchRegression` | the change is slower than the build it is compared with |
| [GIONG-E5065](https://giong.run/docs/errors/E5065.md) | `NoRecording` | no recorded requests to use |
| [GIONG-E5066](https://giong.run/docs/errors/E5066.md) | `RecordedCaseFailed` | a recorded case answers differently |
| [GIONG-E5067](https://giong.run/docs/errors/E5067.md) | `RecordingInvalid` | a recording cannot be read or used |
| [GIONG-E5068](https://giong.run/docs/errors/E5068.md) | `RecordValueMissing` | a recorded case needs a value that is not set |
| [GIONG-E5069](https://giong.run/docs/errors/E5069.md) | `ReplayUnreachable` | the service to replay against does not answer |
| [GIONG-E5070](https://giong.run/docs/errors/E5070.md) | `ImageTarget` | the service cannot be built for Linux here |
| [GIONG-E5071](https://giong.run/docs/errors/E5071.md) | `ImageLibraries` | the shared libraries of the binary cannot be found |
| [GIONG-E5072](https://giong.run/docs/errors/E5072.md) | `ImageNoCerts` | no CA certificates to put in the image |
| [GIONG-E5073](https://giong.run/docs/errors/E5073.md) | `ImageAdd` | a file to add to the image is not usable |
| [GIONG-E5074](https://giong.run/docs/errors/E5074.md) | `ImageLoad` | the image could not be loaded into Docker or Podman |
| [GIONG-E5075](https://giong.run/docs/errors/E5075.md) | `ImagePush` | the image could not be pushed to the registry |
| [GIONG-E5076](https://giong.run/docs/errors/E5076.md) | `ImageTag` | the image name or tag is not valid |
| [GIONG-E5080](https://giong.run/docs/errors/E5080.md) | `FixDirty` | the project has changes that are not committed |
| [GIONG-E5081](https://giong.run/docs/errors/E5081.md) | `FixFailed` | cargo could not apply its fixes |
| [GIONG-E5082](https://giong.run/docs/errors/E5082.md) | `FixConflict` | a fix no longer matches the code |
| [GIONG-E5083](https://giong.run/docs/errors/E5083.md) | `TestFailed` | a test failed |
| [GIONG-E5084](https://giong.run/docs/errors/E5084.md) | `DriverNotHere` | the tests could not run on every driver |
| [GIONG-E5085](https://giong.run/docs/errors/E5085.md) | `NextestMissing` | cargo-nextest is not installed |
| [GIONG-E5086](https://giong.run/docs/errors/E5086.md) | `TestTimeout` | the tests took longer than the time limit |
| [GIONG-E5087](https://giong.run/docs/errors/E5087.md) | `GitInitFailed` | git init failed in the new project |
| [GIONG-E5088](https://giong.run/docs/errors/E5088.md) | `DotenvBuildSetting` | .env sets a cargo or rustc setting |
| [GIONG-E5090](https://giong.run/docs/errors/E5090.md) | `GiongOutdated` | a newer giong release is out |
| [GIONG-E5091](https://giong.run/docs/errors/E5091.md) | `GiongMixed` | the project's giong crates are on different releases |
| [GIONG-E5092](https://giong.run/docs/errors/E5092.md) | `CliOutdated` | this giong command is older than the newest release |
| [GIONG-E5093](https://giong.run/docs/errors/E5093.md) | `UpgradeTarget` | the version to move to is not a release giong upgrade can move to |
| [GIONG-E5094](https://giong.run/docs/errors/E5094.md) | `MsrvBelowTarget` | the project's rust-version is below what the release needs |
| [GIONG-E5095](https://giong.run/docs/errors/E5095.md) | `PinnedVersion` | a giong dependency is pinned to a version |
| [GIONG-E5096](https://giong.run/docs/errors/E5096.md) | `SelfUpdateFailed` | giong upgrade --self could not replace this command |
| [GIONG-E5097](https://giong.run/docs/errors/E5097.md) | `ReleaseUnknown` | the newest giong release could not be looked up |
| [GIONG-E5098](https://giong.run/docs/errors/E5098.md) | `ManifestShape` | a giong dependency is written in a form giong upgrade cannot edit |
