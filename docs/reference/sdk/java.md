# Java SDK

A Java Client for GreptimeDB, which is compatible with GreptimeDB protocol and lightweight.

## Features

- SPI-based extensible network transport layer; provides the default implementation by using the
gRPC framework
- Non-blocking, purely asynchronous API, easy to use
- Automatically collects various performance metrics by default. Users can then configure them and
write to local files
- Users can take in-memory snapshots of critical objects, configure them, and write to local files.
This is helpful when troubleshooting complex issues

## How to use

Please refer to User Guide chapter to learn [how to insall SDK](/user-guide/clients/grpc.md#java), [write data](/user-guide/write-data/grpc.md#java) and [query data](/user-guide/query-data/grpc.md#java).

## Global Options (System properties / Java -Dxxx)

| Name                                           | Description                                                                                                                                                        |
|:-----------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| greptimedb.use\_os\_signal                     | Whether or not to use OS Signal, SDK listens for SIGUSR2 signals by default and can outputs some information. This is helpful when troubleshooting complex issues. |
| greptimedb.signal.out\_dir                     | Signal handler can output to the specified directory, default is the process start directory.                                                                      |
| greptimedb.available\_cpus                     | Specify the number of available cpus, the default is to use the full number of cpus of the current environment.                                                    |
| greptimedb.reporter.period\_minutes            | Metrics reporter timed output period, default 30 minutes.                                                                                                          |
| greptimedb.read.write.rw_logging               | Whether to print logs for each read/write operation, default off.                                                                                                  |                                                                                                          |
## Metrics&Display

At runtime, users can use the SIGUSR2 signal of the Linux platform to output
the status information (display) of the node and the metrics.

### How

```shell
kill -s SIGUSR2 pid
```

The relevant information is output to the specified directory.

By default, 2 files are generated in the program's working directory
(cwd: `lsof -p $pid | grep cwd`)

- greptimedb\_client\_metrics.log.xxx: It records all metrics information for the current
client node
- greptimedb\_client\_display.log.xxx: It records important memory state information about the
current client

### List of Metrics (constantly updated)

| Name                                                            | Description                                                                                                                    |
|:----------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|
| thread\_pool.${thread\_pool\_name} [timer]                      | Thread pool execution task time statistics.                                                                                    |
| scheduled\_thread\_pool.${schedule\_thread\_pool\_name} [timer] | Schedule thread pool execution task time statistics.                                                                           |
| async\_write\_pool.time [timer]                                 | Asynchronous pool time statistics for asynchronous write tasks in SDK, this is important and it is recommended to focus on it. |
| async\_read\_pool.time [timer]                                  | Asynchronous pool time statistics for asynchronous read tasks in SDK, this is important and it is recommended to focus on it.  |
| write\_rows\_success\_num [histogram]                           | Statistics on the number of successful writes.                                                                                 |
| write\_rows\_failure\_num [histogram]                           | Statistics on the number of data entries that failed to write.                                                                 |
| write\_failure\_num [meter]                                     | Statistics on the number of failed writes.                                                                                     |
| write\_qps [meter]                                              | Write Request QPS                                                                                                              |
| write\_by\_retries\_${n} [meter]                                | QPS for the nth retry write, n == 0 for the first write (non-retry), n > 3 will be counted as n == 3                           |
| read\_rows\_num [histogram]                                     | Statistics of the number of data items per query.                                                                              |
| read\_failure\_num [meter]                                      | Statistics of the number of failed queries.                                                                                    |
| serializing\_executor\_single\_task\_timer\_${name} [timer]     | Serializing executor. Single task execution time consumption statistics                                                        |
| serializing\_executor\_drain\_timer\_${name} [timer]            | Serializing executor. Drains all tasks for time consumption statistics                                                         |
| serializing\_executor\_drain\_num\_${name} [histogram]          | Serializing executor. Statistics on the number of draining tasks                                                               |

## Magic Tools

### How to use `kill -s SIGUSR2 $pid`

The first time you execute `kill -s SIGUSR2 $pid` you will see the following help messages on
the log output, including:

- Turn on/off the output of the condensed version of the read/write log.
- Turn on/off limter
- Export in-memory metrics and memory state information of important objects to a local file

### Just follow the help information

``` text
 - -- GreptimeDBClient Signal Help --
 -     Signal output dir: /Users/xxx/xxx
 -
 -     How to open or close read/write log(The second execution means close):
 -       [1] `cd /Users/xxx/xxx`
 -       [2] `touch rw_logging.sig`
 -       [3] `kill -s SIGUSR2 $pid`
 -       [4] `rm rw_logging.sig`
 -
 -     How to get metrics and display info:
 -       [1] `cd /Users/xxx/xxx`
 -       [2] `rm *.sig`
 -       [3] `kill -s SIGUSR2 $pid`
 -
 -     The file signals that is currently open:
 -       rw_logging.sig
 -
```

## Usage Examples
For fully runnable code snippets and explanations for common methods, see the [Usage Examples](https://github.com/GreptimeTeam/greptimedb-client-java/tree/main/greptimedb-example/src/main/java/io/greptime/example).

## API

Please refer to [API Document](https://javadoc.io/doc/io.greptime/greptimedb-protocol/latest/index.html).
