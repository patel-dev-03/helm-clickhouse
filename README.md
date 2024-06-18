# ClickHouse Kubernetes Deployment

This repository contains a Helm chart for deploying a ClickHouse server on a Kubernetes cluster.

## Chart Details

This chart will do the following:

- Create a Deployment for ClickHouse server.
- Create a Service to expose the ClickHouse server.
- Create Persistent Volume Claims for hot and cold storage.
- Create a ConfigMap for ClickHouse server configuration.

## Installing the Chart

To install the chart with the release name `my-release`: to give another change `my-release`

```sh
helm install my-release ./clickhouse
```
## Running ClickHouse Client on Pods

After the ClickHouse server is up and running, you can run the ClickHouse client on a pod using the following command:

Please replace `<pod-name>` with the name of your ClickHouse pod.
```sh
kubectl exec -it <pod-name> -- clickhouse-client
```

## Please change the  config.xml  inside configmap.yaml according to your requirement: 
default - move-factor = 0.2 , but for testing purpose keep the move-factor in range of : 0.8-0.95 to see immediate change of hot->cold storage 


```sh
SELECT *
FROM system.storage_policies
```

Test - table `dz_test`
```sh
CREATE TABLE dz_test
(
    `B` Int64,
    `T` String,
    `D` Date
)
ENGINE = MergeTree
PARTITION BY D
ORDER BY B
SETTINGS storage_policy = 'hot_to_cold';
```

Inserting data for test (to see immediate hot to cold transfer use move factor between (0.8) - (0.95))

```sh
insert into dz_test select number, number, '2023-01-01' from numbers(1e9)
```

## check the cold-storage-size : 

After inserting data beyond threshold the data will start transfering to cold-storage you can check by this command: 

Please replace `<pod-name>` with the name of your ClickHouse pod.
```sh
kubectl exec -it <pod-name> -- du -sh var/lib/clickhouse/cold
```

For hot folder  : 
```sh
kubectl exec -it <pod-name> -- du -sh var/lib/clickhouse/store
```

## get all the images in testing folder

