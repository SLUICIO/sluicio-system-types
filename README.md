# Sluicio system types

Community-shared **system types** for [Sluicio](https://sluicio.com) —
portable monitoring definitions for the brokers, gateways, and runtimes
that carry your integrations. Each file bundles what Sluicio needs to
recognise a system and watch it: metric-name **detection prefixes** and
a set of **starter health checks** (metric thresholds, log matchers,
failed-trace / latency / traffic dead-man rules).

One YAML file. Export from your cell, import into any other. Apache-2.0.

## Use a type from this repo

Download the `.systemtype.yaml` you want from [`types/`](types/), then
either:

- **UI**: System types → **Import…** → pick the file, or
- **API**:

  ```sh
  curl -X POST https://your-cell.example.com/api/v1/system-types/import \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/yaml" \
    --data-binary @rabbitmq.systemtype.yaml
  ```

Importing a key that matches a Sluicio built-in creates your org's
**override** of it; a new key becomes a custom type. Add `?replace=true`
to update a type you imported before.

## Share your own

1. Build your type in Sluicio (System types → New, or fork a built-in
   and tune the checks on a real system).
2. System types → **Export** — you get `<key>.systemtype.yaml`.
3. Open a PR adding it to `types/`.

Guidelines:

- Name the file `<key>.systemtype.yaml`; keys are lowercase
  (`[a-z0-9._-]`, max 63 chars).
- Add a short comment header: what it monitors, which versions you
  tested against, anything a stranger should know before trusting the
  thresholds.
- No credentials, internal hostnames, or company-specific values —
  the file is meant to travel.
- Thresholds are starting points, not gospel — say so in the header if
  yours are opinionated.

The full format specification lives in the app repo:
[docs/system-types-sharing.md](https://github.com/SLUICIO/sluicio-app/blob/main/docs/system-types-sharing.md).

## Included types

| Type | File | What it watches |
|---|---|---|
| RabbitMQ | [`types/rabbitmq.systemtype.yaml`](types/rabbitmq.systemtype.yaml) | Queue depth, consumers, delivery health |
| ActiveMQ Artemis | [`types/artemis.systemtype.yaml`](types/artemis.systemtype.yaml) | Address/queue metrics, broker health |
| KrakenD | [`types/krakend.systemtype.yaml`](types/krakend.systemtype.yaml) | Gateway 5xx/error traces, backend transport failures & timeouts |
| Azure Service Bus | [`types/azure-servicebus.systemtype.yaml`](types/azure-servicebus.systemtype.yaml) | Per-queue/subscription depth and DLQ metrics |
| OTel Collector | [`types/otel-collector.systemtype.yaml`](types/otel-collector.systemtype.yaml) | Pipeline throughput, refused/dropped telemetry |
| .NET service | [`types/dotnet-service.systemtype.yaml`](types/dotnet-service.systemtype.yaml) | Runtime counters, error rates |
| Apache Kafka | [`types/kafka.systemtype.yaml`](types/kafka.systemtype.yaml) | Consumer-group lag, empty groups, in-sync replicas, broker count |
| Confluent Kafka | [`types/confluent-kafka.systemtype.yaml`](types/confluent-kafka.systemtype.yaml) | Consumer lag, cluster load, hot partitions (Confluent Cloud Metrics API) |
| NATS | [`types/nats.systemtype.yaml`](types/nats.systemtype.yaml) | Slow consumers, client connections, server memory |
| Debezium | [`types/debezium.systemtype.yaml`](types/debezium.systemtype.yaml) | Connector connectivity, CDC lag, event-queue capacity |
| WSO2 API Manager | [`types/wso2-apim.systemtype.yaml`](types/wso2-apim.systemtype.yaml) | Failed invocations, gateway latency, dead-man, error logs, JVM heap |
| WSO2 API Manager | [`types/wso2-apim.systemtype.yaml`](types/wso2-apim.systemtype.yaml) | Failed/slow/silent gateway traces, error-log spikes, JVM heap |

These mirror the built-ins shipped with Sluicio, exported verbatim —
fork away.
