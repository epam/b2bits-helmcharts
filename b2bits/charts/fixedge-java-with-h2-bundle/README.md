# FIXEdge Java with H2 and FixEye Agent Bundle on Kubernetes Helm Chart

This is the Official EPAM B2Bits Helm chart for installing and configuring FIXEdge Java, FIXICC H2, and FixEye Agent as a single bundle on Kubernetes.

[Overview of EPAM B2Bits FIXEdge®](https://www.b2bits.com/trading_solutions/fixedge-java)
[Overview of EPAM B2Bits FIXICC®](https://www.b2bits.com/trading_solutions/fixicc)
[Overview of EPAM B2Bits FixEye®](https://www.b2bits.com/trading_solutions/fixeye)

## Quickstart

```bash
$ kubectl create namespace fixedge-java-bundle
$ kubectl create secret generic fixaj2-java-license-file --from-file=path/to/fixaj2-license.bin -n fixedge-java-bundle
$ kubectl create secret generic fixedge-java-license-file --from-file=path/to/fixedgej-license.bin -n fixedge-java-bundle
$ kubectl create secret generic fixeye-license-file --from-file=path/to/fixeye-agent.license -n fixedge-java-bundle
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install fixedge-java-bundle b2bits/fixedge-java-with-h2-bundle -n fixedge-java-bundle
```

## Introduction

This chart bootstraps a complete FIX trading solution deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager, including:

- **[FIXEdge®](https://www.b2bits.com/trading_solutions/fixedge-java)** - FIX protocol engine (Java version)
- **[FIXICC®](https://www.b2bits.com/trading_solutions/fixicc)** - FIX message database with H2
- **[FixEye®](https://www.b2bits.com/trading_solutions/fixeye)** - FIX message monitoring and analysis

All components are deployed in a single pod for optimal performance and data sharing.

## Prerequisites

- Kubernetes 1.21+
- Helm 3.9.0+
- PV provisioner support in the underlying infrastructure
- Valid FIXEdge Java License Key
- Valid FixEye License Key
- Minimum 2.0 CPU cores and 1.5GB RAM available on the target node

## Obtaining License Keys

To run this bundle you will need valid license key files for both FIXEdge Java and FixEye:

- **FIXEdge Java License**: Contact [sales@btobits.com](mailto:sales@b2bits.com)
- **FixEye License**: Contact [sales@btobits.com](mailto:sales@b2bits.com)

## Installing the Chart

To install the chart with the release name `fixedge-java-bundle`:

```bash
$ kubectl create namespace fixedge-java-bundle
$ kubectl create secret generic fixaj2-java-license-file --from-file=path/to/fixaj2-license.bin -n fixedge-java-bundle
$ kubectl create secret generic fixedge-java-license-file --from-file=path/to/fixedgej-license.bin -n fixedge-java-bundle
$ kubectl create secret generic fixeye-license-file --from-file=path/to/fixeye-agent.license -n fixedge-java-bundle
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install fixedge-java-bundle b2bits/fixedge-java-with-h2-bundle -n fixedge-java-bundle
```

These commands deploy the complete B2Bits FIX trading solution on the Kubernetes cluster in the default configuration. The **Parameters** section below lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `fixedge-java-bundle` deployment:

```bash
$ helm delete fixedge-java-bundle -n fixedge-java-bundle
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

| Parameter                                 | Description                                                      | Default                                                  |
| :---------------------------------------- | :--------------------------------------------------------------- | :------------------------------------------------------- |
| force_init_configs                        | Update config on re-deploy                                       | false                                                    |
| git_configs.url                           | Repository with configuration for all components                | https://github.com/epam/b2bits-configuration-samples.git |
| git_configs.branch                        | Branch from the config repository                                | main                                                     |
| imagePullSecrets                          | The secret to downloading an image from a private repository     | []                                                       |
| **FIXEdge Java Parameters**               |                                                                  |                                                          |
| fixedge_java.image.url                    | Repository with fixedge-java image                              | b2bitsepam/fixedge-java                                  |
| fixedge_java.image.version                | Version of the fixedge-java image                               | latest                                                   |
| fixedge_java.image.imagePullPolicy        | Image policy pull options                                        | Always                                                   |
| fixedge_java.port                         | Application port                                                 | 8911                                                     |
| fixedge_java.httpAdmPort                   | Admin application port                                           | 9010                                                     |
| fixedge_java.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 60                                                       |
| fixedge_java.livenessProbe.periodSeconds   | How often (in seconds) to perform the probe                      | 30                                                       |
| fixedge_java.livenessProbe.timeoutSeconds | Number of seconds after which the probe times out                | 5                                                        |
| fixedge_java.livenessProbe.failureThreshold | Minimum consecutive failures for the probe to be considered failed | 3                                                        |
| fixedge_java.readinessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 60                                                       |
| fixedge_java.readinessProbe.periodSeconds | How often (in seconds) to perform the probe                      | 30                                                       |
| fixedge_java.readinessProbe.timeoutSeconds | Number of seconds after which the probe times out                | 5                                                        |
| fixedge_java.readinessProbe.failureThreshold | Minimum consecutive failures for the probe to be considered failed | 3                                                        |
| fixedge_java.resources                    | CPU/Memory resource requests/limits                              | Memory: 1Gi, CPU: 500m                                   |
| fixedge_java.storage.class                | Storage class name                                               | gp2                                                      |
| fixedge_java.storage.accessModes          | Access Mode for storage class                                    | ReadWriteOnce                                            |
| fixedge_java.storage.fe_java_configs.size | Storage size                                                     | 1Gi                                                      |
| fixedge_java.storage.fe_java_sessions_logs.size | Storage size                                                     | 10Gi                                                     |
| fixedge_java.storage.fe_java_app_logs.size | Storage size                                                     | 10Gi                                                     |
| **FIXICC Parameters**                     |                                                                  |                                                          |
| fixicc.image.url                          | Repository with fixicc image                                     | b2bitsepam/fixicc-h2                                     |
| fixicc.image.version                      | Version of the fixicc image                                      | latest                                                   |
| fixicc.image.imagePullPolicy             | Image policy pull options                                        | Always                                                   |
| fixicc.port                               | Application port                                                 | 8080                                                     |
| fixicc.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 60                                                       |
| fixicc.livenessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 30                                                       |
| fixicc.livenessProbe.timeoutSeconds      | Number of seconds after which the probe times out                | 10                                                       |
| fixicc.livenessProbe.failureThreshold    | Minimum consecutive failures for the probe to be considered failed | 5                                                        |
| fixicc.readinessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 30                                                       |
| fixicc.readinessProbe.periodSeconds      | How often (in seconds) to perform the probe                      | 15                                                       |
| fixicc.readinessProbe.timeoutSeconds     | Number of seconds after which the probe times out                | 10                                                       |
| fixicc.readinessProbe.failureThreshold   | Minimum consecutive failures for the probe to be considered failed | 5                                                        |
| fixicc.resources                          | CPU/Memory resource requests/limits                              | Memory: 2Gi, CPU: 1000m                                  |
| fixicc.storage.class                      | Storage class name                                               | gp2                                                      |
| fixicc.storage.accessModes               | Access Mode for storage class                                    | ReadWriteOnce                                            |
| fixicc.storage.fixicc_configs.size       | Storage size                                                     | 1Gi                                                      |
| fixicc.storage.fixicc_logs.size          | Storage size                                                     | 10Gi                                                     |
| **FixEye Parameters**                     |                                                                  |                                                          |
| fixeye.image.url                          | Repository with fixeye image                                     | b2bitsepam/fixeye-agent                                  |
| fixeye.image.version                      | Version of the fixeye image                                      | latest                                                   |
| fixeye.image.imagePullPolicy             | Image policy pull options                                        | Always                                                   |
| fixeye.port                               | Application port                                                 | 8882                                                     |
| fixeye.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 15                                                       |
| fixeye.livenessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 20                                                       |
| fixeye.readinessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 5                                                        |
| fixeye.readinessProbe.periodSeconds      | How often (in seconds) to perform the probe                      | 10                                                       |
| fixeye.readinessProbe.timeoutSeconds     | Number of seconds after which the probe times out                | 3                                                        |
| fixeye.readinessProbe.failureThreshold   | Minimum consecutive failures for the probe to be considered failed | 3                                                        |
| fixeye.resources                          | CPU/Memory resource requests/limits                              | Memory: 500Mi, CPU: 500m                                 |
| fixeye.storage.class                      | Storage class name                                               | gp2                                                      |
| fixeye.storage.accessModes               | Access Mode for storage class                                    | ReadWriteOnce                                            |
| fixeye.storage.configs.size               | Storage size                                                     | 1Gi                                                      |
| fixeye.storage.samples.size               | Storage size                                                     | 1Gi                                                      |
| fixeye.storage.logs.size                  | Storage size                                                     | 10Gi                                                     |

## Architecture

The chart deploys a single StatefulSet containing three main containers:

1. **FIXEdge Java Container**: Runs the FIX protocol engine
2. **FIXICC H2 Container**: Provides FIX message database functionality
3. **FixEye Agent Container**: Monitors and analyzes FIX messages

Additionally, there are init containers for configuration setup and a clean-archive container for log maintenance.

## Data Sharing

The FixEye Agent container mounts the FIXEdge Java session logs volume at `/usr/share/doc/fixeye/samples` in read-only mode, allowing it to access and analyze FIX message data.

## Health Checks

- **FIXEdge Java**: HTTP GET to `/service/started` on port 9010 (HTTPS)
- **FIXICC**: HTTP GET to `/app` on port 8080
- **FixEye**: TCP socket check on port 8882

## Troubleshooting

For troubleshooting and support, please contact [SupportFIXAntenna@epam.com](mailto:SupportFIXAntenna@epam.com)
