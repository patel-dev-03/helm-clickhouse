# ClickHouse Kubernetes Deployment

This repository contains a Helm chart for deploying a ClickHouse server on a Kubernetes cluster.

## Chart Details

This chart will do the following:

- Create a Deployment for ClickHouse server.
- Create a Service to expose the ClickHouse server.
- Create Persistent Volume Claims for hot and cold storage.
- Create a ConfigMap for ClickHouse server configuration.
