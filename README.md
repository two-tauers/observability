# Observability setup for two-tauers


## OpenTelemetry

OpenTelemetry agents running as a daemonset to collect and forward metrics and logs to Grafana Cloud.

>NOTE: There is a bug in loki exporter with a not yet released fix: https://stackoverflow.com/questions/73946449/unable-to-map-otlp-resource-attributes-into-loki-labels

### Install

1. Create `opentelemetry/endpoints.yaml` file with the Prometheus and Loki endpoints for your Grafana Cloud instance, e.g.:

    ```yaml
    ---
    config:
    exporters:
        prometheusremotewrite:
        endpoint: https://<USER>:<TOKEN>@prometheus-prod-10-prod-us-central-0.grafana.net/api/prom/push

        loki:
        endpoint: "https://<USER>:<TOKEN>@logs-prod3.grafana.net/loki/api/v1/push"
    ```

2. Install the helm chart by running

    ```bash
    helm upgrade --install otel-agent opentelemetry/opentelemetry-collector -n opentelemetry --create-namespace --values opentelemetry/agent.yaml --values opentelemetry/endpoints.yaml
    ```

### What is collected

#### Logs

STDOUT and STDERR streams from pods running in the cluster.

#### Metrics

TBD (Currently only collects its own metrics)

### Dashboard

https://grafana.com/grafana/dashboards/15983-opentelemetry-collector/
