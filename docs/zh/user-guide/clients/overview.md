# 概述

你可以使用多种协议从 GreptimeDB 中读取或写入数据。

![protocols](../../../public/b8fade22-59b2-42a8-aab9-a79cdca36d27.png)

请注意，使用特定协议写入数据并不意味着你必须使用相同的协议读取数据。例如，你可以通过 gRPC 端点写入数据，同时使用 MySQL 客户端读取它们。

## 鉴权

请参阅 [鉴权](./authentication.md) 以了解如何为 GreptimeDB 配置用户名和密码。

## 客户端协议

- [MySQL](./mysql.md)
- [gRPC](./grpc.md)
- [InfluxDB line](./influxdb-line.md)
- [OpenTSDB](./opentsdb.md)
- [PostgreSQL](./postgresql.md)
- [HTTP API](./http-api.md)
