# FIXEdge CPP with H2 and FixEye Agent Bundle on Kubernetes Helm Chart

This is the Official EPAM B2Bits Helm chart for installing and configuring FIXEdge CPP, FIXICC H2, and FixEye Agent as a single bundle on Kubernetes.

[Overview of EPAM B2Bits FIXEdge®](https://www.b2bits.com/trading_solutions/fixedge)
[Overview of EPAM B2Bits FIXICC®](https://www.b2bits.com/trading_solutions/fixicc)
[Overview of EPAM B2Bits FixEye®](https://www.b2bits.com/trading_solutions/fixeye)

## Quickstart

```bash
$ kubectl create namespace fixedge-cpp-bundle
$ kubectl create secret generic license-file --from-file=path/to/engine.license -n fixedge-cpp-bundle
$ kubectl create secret generic fixeye-license-file --from-file=path/to/fixeye-agent.license -n fixedge-cpp-bundle
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install fixedge-cpp-bundle b2bits/fixedge-cpp-with-h2-bundle -n fixedge-cpp-bundle
```

## Introduction

This chart bootstraps a complete FIX trading solution deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager, including:

- **[FIXEdge®](https://www.b2bits.com/trading_solutions/fixedge)** - FIX protocol engine
- **[FIXICC®](https://www.b2bits.com/trading_solutions/fixicc)** - FIX message database with H2
- **[FixEye®](https://www.b2bits.com/trading_solutions/fixeye)** - FIX message monitoring and analysis

All components are deployed in a single pod for optimal performance and data sharing.

## Prerequisites

- Kubernetes 1.21+
- Helm 3.9.0+
- PV provisioner support in the underlying infrastructure
- Valid FIXEdge License Key
- Valid FixEye License Key
- Minimum 2.0 CPU cores and 1.5GB RAM available on the target node

## Obtaining License Keys

To run this bundle you will need valid license key files for both FIXEdge and FixEye:

- **FIXEdge License**: Contact [sales@btobits.com](mailto:sales@b2bits.com)
- **FixEye License**: Contact [sales@btobits.com](mailto:sales@b2bits.com)

## Installing the Chart

To install the chart with the release name `fixedge-cpp-bundle`:

```bash
$ kubectl create namespace fixedge-cpp-bundle
$ kubectl create secret generic license-file --from-file=path/to/engine.license -n fixedge-cpp-bundle
$ kubectl create secret generic fixeye-license-file --from-file=path/to/fixeye-agent.license -n fixedge-cpp-bundle
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install fixedge-cpp-bundle b2bits/fixedge-cpp-with-h2-bundle -n fixedge-cpp-bundle
```

These commands deploy the complete B2Bits FIX trading solution on the Kubernetes cluster in the default configuration. The **Parameters** section below lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `fixedge-cpp-bundle` deployment:

```bash
$ helm delete fixedge-cpp-bundle -n fixedge-cpp-bundle
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

| Parameter                                 | Description                                                      | Default                                                  |
| :---------------------------------------- | :--------------------------------------------------------------- | :------------------------------------------------------- |
| force_init_configs                        | Update config on re-deploy                                       | false                                                    |
| git_configs.url                           | Repository with configuration for all components                | https://github.com/epam/b2bits-configuration-samples.git |
| git_configs.branch                        | Branch from the config repository                                | main                                                     |
| imagePullSecrets                          | The secret to downloading an image from a private repository     | []                                                       |
| **FIXEdge Parameters**                    |                                                                  |                                                          |
| fixedge.image.url                         | Repository with fixedge image                                    | b2bitsepam/fixedge-cpp                                   |
| fixedge.image.version                     | Version of the fixedge image                                     | latest                                                   |
| fixedge.image.imagePullPolicy             | Image policy pull options                                        | Always                                                   |
| fixedge.port                              | Application port                                                 | 8901                                                     |
| fixedge.httpAdmPort                       | Admin application port                                           | 8903                                                     |
| fixedge.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 15                                                       |
| fixedge.livenessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 20                                                       |
| fixedge.resources                         | CPU/Memory resource requests/limits                              | Memory: 500Mi, CPU: 500m                                 |
| fixedge.storage.class                     | Storage class name                                               | gp2                                                      |
| fixedge.storage.accessModes               | Access Mode for storage class                                    | ReadWriteOnce                                            |
| fixedge.fe_configs.size                   | Storage size                                                     | 1Gi                                                      |
| fixedge.fe_sessions_logs.size             | Storage size                                                     | 10Gi                                                     |
| fixedge.fe_app_logs.size                  | Storage size                                                     | 10Gi                                                     |
| **FIXICC Parameters**                     |                                                                  |                                                          |
| fixicc.image.url                          | Repository with fixicc image                                     | b2bitsepam/fixicc-h2                                     |
| fixicc.image.version                      | Version of the fixicc image                                      | latest                                                   |
| fixicc.image.imagePullPolicy             | Image policy pull options                                        | Always                                                   |
| fixicc.port                               | Application port                                                 | 8080                                                     |
| fixicc.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 60                                                       |
| fixicc.livenessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 30                                                       |
| fixicc.resources                          | CPU/Memory resource requests/limits                              | Memory: 2Gi, CPU: 1000m                                  |
| fixicc.storage.class                      | Storage class name                                               | gp2                                                      |
| fixicc.storage.accessModes                | Access Mode for storage class                                     | ReadWriteOnce                                            |
| fixicc.fixicc_configs.size                | Storage size                                                     | 1Gi                                                      |
| fixicc.fixicc_logs.size                   | Storage size                                                     | 10Gi                                                     |
| **FixEye Parameters**                     |                                                                  |                                                          |
| fixeye.image.url                          | Repository with fixeye image                                     | b2bitsepam/fixeye-agent                                  |
| fixeye.image.version                      | Version of the fixeye image                                      | latest                                                   |
| fixeye.image.imagePullPolicy             | Image policy pull options                                        | Always                                                   |
| fixeye.port                               | Application port                                                 | 8882                                                     |
| fixeye.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 15                                                       |
| fixeye.livenessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 20                                                       |
| fixeye.resources                          | CPU/Memory resource requests/limits                              | Memory: 500Mi, CPU: 500m                                 |
| fixeye.storage.class                      | Storage class name                                               | gp2                                                      |
| fixeye.storage.accessModes                | Access Mode for storage class                                     | ReadWriteOnce                                            |
| fixeye.configs.size                       | Storage size                                                     | 1Gi                                                      |
| fixeye.samples.size                       | Storage size                                                     | 1Gi                                                      |
| fixeye.logs.size                          | Storage size                                                     | 10Gi                                                     |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install fixedge-cpp-bundle --set force_init_configs=true,fixedge.resources.limits.memory=256Mi b2bits/fixedge-cpp-with-h2-bundle -n fixedge-cpp-bundle
```

The above command forces pull configuration on start (`force_init_configs=true`) and sets FIXEdge RAM limit to `256Mi`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install fixedge-cpp-bundle -f values.yaml b2bits/fixedge-cpp-with-h2-bundle -n fixedge-cpp-bundle
```

> **Tip**: You can use the default [values.yaml](values.yaml) for reference.

## Configuration and installation details

### Using custom configuration

By default this helm chart clones configuration from `https://github.com/epam/b2bits-configuration-samples/tree/main/fixedge-cpp-with-h2-bundle/` with the following structure:

```
fixedge-cpp-with-h2-bundle/
├── fixedge-cpp/
├── fixicc-h2/
└── fixeye-agent/
```

If you choose to maintain your custom configuration in private Git repository, you should provide SSH details for access to remote repository:

```bash
$ kubectl create secret generic ssh-creds --from-file=path/to/known_hosts --from-file=path/to/id_rsa -n fixedge-cpp-bundle
```

```bash
$ helm install fixedge-cpp-bundle --set git_configs.url=<your-git-repo-ssh-url>,git_configs.branch=<your-git-repo-branch> b2bits/fixedge-cpp-with-h2-bundle -n fixedge-cpp-bundle
```

### Component Integration

This bundle provides seamless integration between components:

- **FIXEdge** processes FIX messages and stores session data in `/var/lib/fixedge`
- **FIXICC** provides database functionality for FIX message storage and retrieval
- **FixEye** monitors and analyzes FIX messages, with read access to FIXEdge data via shared volume mount

## Persistence

Each component has its own persistent storage:

- **FIXEdge**: 
  - `/etc/fixedge` (configs) - 1Gi
  - `/var/lib/fixedge` (session logs) - 10Gi
  - `/var/log/fixedge` (application logs) - 10Gi
- **FIXICC**: 
  - `/etc/fixicc-h2` (configs) - 1Gi
  - `/var/log/fixicc-h2` (logs) - 10Gi
- **FixEye**: 
  - `/etc/fixeye` (configs) - 1Gi
  - `/usr/share/doc/fixeye/samples` (samples) - 1Gi
  - `/var/log/fixeye` (logs) - 10Gi

This is known to work in AWS, minikube and k3s. See the [Parameters](#parameters) section to configure the PVCs.

## Services

The chart creates three services for component access:

- **fixedge**: Exposes FIXEdge ports (8901, 8903)
- **fixicc**: Exposes FIXICC web interface (8080)
- **fixeye**: Exposes FixEye agent (8882)

## Resource Requirements

Default resource allocation:

- **FIXEdge**: 0.5 CPU, 500Mi memory (limits: 0.5 CPU, 500Mi)
- **FIXICC**: 1.0 CPU, 2Gi memory (limits: 1.0 CPU, 2Gi)
- **FixEye**: 0.5 CPU, 500Mi memory (limits: 0.5 CPU, 500Mi)
- **Total requests**: 2.0 CPU, 3Gi memory

For production deployments, ensure your nodes have sufficient resources (minimum 2.0 CPU cores and 1.5GB RAM).

## Where to get help

### Sales Information

Email: [sales@btobits.com](mailto:sales@b2bits.com)

### Technical Queries

For technical queries please contact [SupportFIXAntenna@epam.com](mailto:SupportFIXAntenna@epam.com)

## License

Copyright © B2BITS EPAM Systems Company 2000-2023

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
