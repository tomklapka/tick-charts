# Chronograf

##  An Open-Source Time Series Visualization Tool

[Chronograf](https://github.com/influxdata/chronograf) is an open-source web application built by the folks over at [InfluxData](https://influxdata.com) and written in Go and React.js that provides the tools to visualize your monitoring data and easily create alerting and automation rules.

## QuickStart

```bash
$ helm install stable/chronograf --name foo --namespace bar
```

## Introduction

This chart bootstraps a Chronograf deployment and service on a Kubernetes cluster using the Helm Package manager.

## Prerequisites

- Kubernetes 1.4+
- PV provisioner support in the underlying infrastructure (optional)

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release stable/chronograf
```

The command deploys Chronograf on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release --purge
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The configurable parameters of the Chronograf chart and 
their descriptions can be seen in `values.yaml`. The [full image documentation](https://quay.io/influxdb/chronograf) contains more information about running Chronograf in docker.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  --set ingress.enabled=true,ingress.hostname=chronograf.foobar.com \
    stable/chronograf
```

The above command enables persistence and changes the size of the requested data volume to 200GB.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml stable/chronograf
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The [Chronograf](https://quay.io/influxdb/chronograf) image stores data in the `/var/lib/chronograf` directory in the container.

The chart optionally mounts a [Persistent Volume](kubernetes.io/docs/user-guide/persistent-volumes/) volume at this location. The volume is created using dynamic volume provisioning.

## OAuth Using Kubernetes Secret

OAuth, among other things, can be configured in Chronograf using environment variables. For more information please see https://docs.influxdata.com/chronograf/latest/administration/managing-security

Taking Google as an example, to use an existing Kubernetes Secret that contains sensitive information (`GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET`), e.g.:

```
apiVersion: v1
kind: Secret
metadata:
  name: chronograf-google-env-secrets
  namespace: tick
type: Opaque
data:
    GOOGLE_CLIENT_ID: <BASE64_ENCODED_GOOGLE_CLIENT_ID>
    GOOGLE_CLIENT_SECRET: <BASE64_ENCODED_GOOGLE_CLIENT_SECRET>
```

in conjunction with less sensitive information such as `GOOGLE_DOMAINS` and `PUBLIC_URL`, one can make use of the chart's `envFromSecret` and `env` values, e.g. a values file can have the following:

```
[...]
env:
  GOOGLE_DOMAINS: "yourdomain.com"
  PUBLIC_URL: "https://chronograf.yourdomain.com"
envFromSecret: chronograf-google-env-secrets
[...]
```
