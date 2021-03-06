# Elasticsearch

[Elasticsearch](https://www.elastic.co/products/elasticsearch) is a highly scalable open-source full-text search and analytics engine. It allows you to store, search, and analyze big volumes of data quickly and in near real time.

## TL;DR

```console
$ helm install incubator/elasticsearch
```

## Introduction

This chart bootstraps a [Elasticsearch](https://github.com/bitnami/bitnami-docker-elasticsearch) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.6+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release incubator/elasticsearch
```

The command deploys Elasticsearch on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Elasticsearch chart and their default values.

|             Parameter              |                      Description                      |                               Default                               |
|------------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|
| image.name                         | Elasticsearch image name                              | `bitnami/elasticsearch`                                             |
| image.tag                          | Elasticsearch image tag                               | `{VERSION}`                                                         |
| image.pullPolicy                   | Image pull policy                                     | `IfNotPresent`                                                      |
| service.type                       | Kubernetes service type                               | `ClusterIP`                                                         |
| cluster.name                       | Elasticsearch cluster name                            | `elasticsearch-cluster`                                             |
| cluster.port.http                  | Elasticsearch REST port                               | `9200`                                                              |
| node.serviceAccountName            | Kubernetes service account                            | `default`                                                           |
| node.plugins                       | Elasticsearch node plugins                            | `io.fabric8:elasticsearch-cloud-kubernetes:5.5.2` (required plugin) |
| node.config                        | Elasticsearch node custom configuration               | ``                                                                  |
| node.master.name                   | Master node pod name                                  | `master`                                                            |
| node.master.replicas               | Desired number of Elasticsearch master eligible nodes | `2`                                                                 |
| node.master.heapSize               | master node heap size                                 | `128m`                                                              |
| node.master.resources              | CPU/Memory resource requests/limits for master nodes  | `{ memory: "256Mi" }`                                               |
| node.data.name                     | Data node pod name                                    | `data`                                                              |
| node.data.replicas                 | Desired number of Elasticsearch data eligible nodes   | `3`                                                                 |
| node.data.heapSize                 | data node heap size                                   | `1024m`                                                             |
| node.data.resources                | CPU/Memory resource requests/limits for data nodes    | `{ memory: "512Mi" }`                                               |
| node.data.persistence.enabled      | Enable persistence using a `PersistentVolumeClaim`    | `true`                                                              |
| node.data.persistence.annotations  | Persistent Volume Claim annotations                   | `{}`                                                                |
| node.data.persistence.storageClass | Persistent Volume Storage Class                       | ``                                                                  |
| node.data.persistence.accessModes  | Persistent Volume Access Modes                        | `[ReadWriteOnce]`                                                   |
| node.data.persistence.size         | Persistent Volume Size                                | `8Gi`                                                               |

The above parameters map to the env variables defined in [bitnami/elasticsearch](http://github.com/bitnami/bitnami-docker-elasticsearch). For more information please refer to the [bitnami/elasticsearch](http://github.com/bitnami/bitnami-docker-elasticsearch) image documentation.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set cluster.name=elastic,cluster.port.http=8080 \
  incubator/elasticsearch
```

The above command sets the Elasticsearch cluster name to `elastic` and REST port number to `8080`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml incubator/elasticsearch
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The [Bitnami Elasticsearch](https://github.com/bitnami/bitnami-docker-elasticsearch) image stores the Elasticsearch data and configurations at the `/bitnami` path of the container. Persistent Volume Claims are used to persist the state of the data processing Elasticsearch nodes. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.
