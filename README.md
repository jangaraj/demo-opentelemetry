# Demo OpenTelemetry

Notes:
- `OTEL` - OpenTelemetry
- `OTLP` - OpenTelemetry protocol

Single click provisioning 
[![Gitpod ready-to-test](https://img.shields.io/badge/Gitpod-ready--to--test-blue?logo=gitpod)](https://gitpod.io/#https://github.com/jangaraj/demo-opentelemetry/) 
- Keycloak login: `admin/admin`

## Observability data

App may produce these basic types of observability data:

- `metrics`: numeric values with dimensions, they have low cardinality usually, e.g. number of total 5XX responses
- `logs`: unstructured strings, which can be parsed, e.g. access logs, which can be parsed (time, endpoint, response code), so they may be represented as metrics - number of 5XX responses for each endpoint
- `traces`: structured data, captured for selected operation

## Observability data flow

Typical observability data flow:

- generation: app generates observability data (e.g. logs) or 
  it may also expose them for further collection (e.g. metrics via `/metrics` endpoint)
- processing: processing of observability data (e.g. filtering, converting vendor specific data types, ...)
- storing: saving of observability data, there is no storage, which will fit for all 
  observability data types perfectly
- visualization/alerting

## OpenTelemetry

- offers instrumentation for [many popular programming languages](https://opentelemetry.io/docs/instrumentation/)
- it is vendor agnostic and it has support from many vendors
- OTEL collector offers fully configurable pipelines, where user can add 
[receivers](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver), 
[processors](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor), 
[exporters](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter)
- it has native support from AWS, so it can be used in many their solutions (EKS, ECS, Lambda, ...)

## Demo Stack

![Infrastructure](https://raw.githubusercontent.com/jangaraj/demo-opentelemetry/main/doc/diagram.png)

### Metrics

[OTEL Java agent with autoinstrumentation](https://github.com/open-telemetry/opentelemetry-java-instrumentation)
generates OTLP metrics and pushs them to 
[OTEL collector](https://github.com/open-telemetry/opentelemetry-collector-contrib). 
Keycloak exports metrics to `/metrics` endpoint and 
[OTEL collector](https://github.com/open-telemetry/opentelemetry-collector-contrib)
scrapes them.
[OTEL collector](https://github.com/open-telemetry/opentelemetry-collector-contrib)
exports all collected metrics to [Prometheus](https://github.com/prometheus/prometheus)

### Traces

[OTEL Java agent with autoinstrumentation](https://github.com/open-telemetry/opentelemetry-java-instrumentation)
generates traces and sends them to [OTEL collector](https://github.com/open-telemetry/opentelemetry-collector-contrib)
which exports them to [Jaeger](https://github.com/jaegertracing/jaeger)

### Logs

Keycloak generates JSON logs, which are processed by Logspout/Logstash and inserted to Elasticsearch.

### Visualization

All observability sources (metrics, traces, logs) are aggregated/visualized 
in the [Grafana](https://github.com/grafana/grafana).

## OpenTelemetry Demo

https://github.com/open-telemetry/opentelemetry-demo
