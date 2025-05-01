# Monitoring

This project defines a Jenkins pipeline to deploy the monitoring resources on a kubernetes cluster.

## Structure

The project conists of several components:
- Influxdb to store telegraf data.
- Telegraf to gather traefik and nfs metrics.
- Prometheus to gather k8s cluster data.
- Grafana, alloy and loki.

## Secrets

The following credentials must be available on Jenkins:
- server1-domain
- nfs1-server
- influxdb-operator-password
- influxdb-operator-token
- influxdb-telegraf-cp-token
- influxdb-telegraf-nfs1-token
- nfs1-snmp (user & password)
- nfs1-snmp-privacy-password

## Backups

Influxdb & Grafana create daily backups on the NFS server and deletes old backups.
