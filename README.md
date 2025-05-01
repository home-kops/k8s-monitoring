# Monitoring

This project defines a Jenkins pipeline to deploy the monitoring resources on a kubernetes cluster.

## Structure

The project conists of several components:
- Influxdb to store telegraf data.
- Telegraf to gather nfs metrics.
- Prometheus to gather k8s cluster data.
- Grafana, alloy and loki.

## Backups

Influxdb & Grafana create daily backups on the NFS server and deletes old backups.
