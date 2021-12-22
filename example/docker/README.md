# Uptrace Open Source demo

This example demonstrates how to start Uptrace and ClickHouse using Docker. It uses
[uptrace.yml](uptrace.yml) to configure Uptrace.

**Step 1**. Start the services:

```shell
docker-compose up -d
```

**Step 2**. Make sure Uptrace is running:

```shell
$ docker-compose logs uptrace

reading config from ~/uptrace/uptrace.yml
serving on http://localhost:15678/ UPTRACE_DSN=http://localhost:4317 OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4317
```

**Step 3**. Open Uptrace UI at http://localhost:15678

Uptrace will monitor itself using [uptrace-go](https://github.com/uptrace/uptrace-go) OpenTelemetry
distro. To get some test data, just reload the UI few times.

You can also run the [basic](https://github.com/uptrace/uptrace-go/tree/master/example/basic)
example:

```go
UPTRACE_DSN= go run .
```