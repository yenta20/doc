# 9. Monitoring solution.

Date: 2020-11-10

## Status

In progress

## Context

Solution for the system and business metrics, traces, logs and alerting.

Recon monitoring was build on top of GCP:
* Logs: https://console.cloud.google.com/logs/query
* Traces: https://console.cloud.google.com/traces/list
* Metrics: https://console.cloud.google.com/monitoring/dashboards
* Alerts: https://console.cloud.google.com/monitoring/alerting

## Monitoring metrics

#### Business metrics:

* API request rate
* API request count (grouped by endpoint path)
* GEO coding latency (grouped by GEO provider and operation status)
* GEO coding requests and covered points (grouped by GEO provider and operation status)

#### System K8S metrics (grouped by pod):

* CPU utilization
* RAM usage
* Ephemeral storage usage
* Network I/O
* Restarts count
* Containers count

#### System PubSub metrics:

* DLQ message count
* Publish requests count
* Unacknowledged message count

#### System CloudSQL metrics:

* CPU utilization
* RAM usage
* Connections count
* Transactions count (grouped by type)
* Disk I/O
* Network I/O
* Disk usage
* Uptime

#### System Datastore metrics:

* Request count
* Index writes
* Read/Write entities size

#### System Storage metrics (grouped by bucket):

* Object count
* Total bytes

## Monitoring dashboards

Currently, we have one composite dashboard with all metrics inside.

Dashboard management will be implemented via GCP monitoring API:
https://cloud.google.com/monitoring/api/ref_v3/rest/v1/projects.dashboards

## Alert channels

We are going to use `Webhooks` GCP notification channel to send critical alert to specific Telegram group channel

## Alert triggers and notifications

* DLQ message (> 0)
* K8S horizontal pod autoscaling (TBD)
* SQL DB CPU utilization (> 60%)
* SQL DB RAM utilization (> 60%)
* SQL DB rollback transactions (TBD)
* Recon API errors (TBD)
* Recon API latency (TBD)
* Recon GEO errors (TBD)
* Panic count (TBD)
