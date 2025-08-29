# FIXICC H2 on Kubernetes Helm Chart

This is the Official EPAM B2Bits Helm chart for installing and configuring FIXICC H2 on Kubernetes.

[Overview of EPAM B2Bits FIXICC](https://www.b2bits.com/trading_solutions/fixicc-h2)

## Quickstart

```bash
$ kubectl create namespace fixicc-h2
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install my-fixicc b2bits/fixicc-h2 -n fixicc-h2
```

## Introduction

This chart bootstraps a [FIXICC H2](https://www.b2bits.com/trading_solutions/fixicc-h2) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.21+
- Helm 3.9.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-fixicc`:

```bash
$ kubectl create namespace fixicc-h2
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install my-fixicc b2bits/fixicc-h2 -n fixicc-h2
```

These commands deploy B2Bits FIXICC H2 on the Kubernetes cluster in the default configuration. The **Parameters** section below lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-fixicc` deployment:

```bash
$ helm delete my-fixicc -n fixicc-h2
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

| Parameter                                 | Description                                                      | Default                                                  |
| :---------------------------------------- | :--------------------------------------------------------------- | :------------------------------------------------------- |
| force_init_configs                        | Update config on re-deploy                                       | false                                                    |
| git_configs.url                           | Repository with configuration for fixicc                         | https://github.com/epam/b2bits-configuration-samples.git |
| git_configs.branch                        | Branch from the config repository                                | main                                                     |
| imagePullSecrets                          | The secret to downloading an image from a private repository     | []                                                       |
| fixicc.image.url                          | Repository with fixicc image                                     | b2bitsepam/fixicc-h2                                     |
| fixicc.image.version                      | Version of the fixicc image                                      | latest                                                   |
| fixicc.image.imagePullPolicy             | Image policy pull options                                        | Always                                                   |
| fixicc.port                              | Application port                                                 | 8080                                                     |
| fixicc.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 60                                                       |
| fixicc.livenessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 30                                                       |
| fixicc.livenessProbe.timeoutSeconds      | Number of seconds after which the probe times out                | 10                                                       |
| fixicc.livenessProbe.failureThreshold    | Minimum consecutive failures for the probe to be considered failed | 5                                                        |
| fixicc.readinessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 30                                                       |
| fixicc.readinessProbe.periodSeconds       | How often (in seconds) to perform the probe                      | 15                                                       |
| fixicc.readinessProbe.timeoutSeconds      | Number of seconds after which the probe times out                | 10                                                       |
| fixicc.readinessProbe.failureThreshold    | Minimum consecutive failures for the probe to be considered failed | 5                                                        |
| fixicc.resources                          | CPU/Memory resource requests/limits                              | Memory: 2Gi, CPU: 1000m                                 |
| fixicc.storage.class                      | Storage class name                                               | gp2                                                      |
| fixicc.storage.accessModes                | Access Mode for storage class                                    | ReadWriteOnce                                            |
| fixicc.storage.fixicc_configs.size        | Storage size for configuration files                             | 1Gi                                                      |
| fixicc.storage.fixicc_logs.size           | Storage size for application logs                               | 10Gi                                                     |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install my-fixicc --set force_init_configs=true,fixicc.resources.limits.memory=4Gi b2bits/fixicc-h2 -n fixicc-h2
```

The above command forces pull configuration on start (`force_init_configs=true`) and sets RAM limit to `4Gi`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install my-fixicc -f values.yaml b2bits/fixicc-h2 -n fixicc-h2
```

> **Tip**: You can use the default [values.yaml](values.yaml) for reference.

## Configuration and installation details

### Using custom configuration

By default this helm chart clones FIXICC H2 configuration from `https://github.com/epam/b2bits-configuration-samples/tree/main/fixicc-h2`.

If you choose to maintain your custom FIXICC H2 configuration in private Git repository, you should provide SSH details for access to remote repository:

```bash
$ kubectl create secret generic ssh-creds --from-file=path/to/known_hosts --from-file=path/to/id_rsa -n fixicc-h2
```

```bash
$ helm install my-fixicc --set git_configs.url=<your-git-repo-ssh-url>,git_configs.branch=<your-git-repo-branch> b2bits/fixicc-h2 -n fixicc-h2
```

It is expected that all FIXICC H2 configuration files are grouped under `fixicc-h2` folder in your repository.

## Persistence

The [EPAM B2Bits FIXICC H2](https://hub.docker.com/r/b2bitsepam/fixicc-h2) image stores the FIXICC H2 data at the following paths of the container:

- Configuration files: `/etc/fixicc-h2`
- Application logs: `/var/log/fixicc-h2`

Persistent Volume Claims are used to keep the data across deployments. This is known to work in AWS, minikube and k3s.
See the [Parameters](#parameters) section to configure the PVC.

## Access

The FIXICC H2 application is accessible via HTTP on port 8080:
- **URL**: `http://localhost:8080/app`
- **Service**: `fixicc` (ClusterIP)

To access the application from outside the cluster, you can use port-forwarding:

```bash
$ kubectl port-forward -n fixicc-h2 svc/fixicc 8080:8080
```

Then open your browser and navigate to `http://localhost:8080/app`

## Where to get help

### Sales Information

Email: [sales@btobits.com](mailto:sales@b2bits.com)

### Technical Queries

For technical queries please contact [SupportFIXAntenna@epam.com](mailto:SupportFIXAntenna@epam.com)

## License

Copyright © B2BITS EPAM Systems Company 2000-2023

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
