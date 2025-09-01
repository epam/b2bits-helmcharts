# FixEye Agent on Kubernetes Helm Chart

This is the Official EPAM B2Bits Helm chart for installing and configuring FixEye Agent on Kubernetes.

[Overview of EPAM B2Bits FixEye Agent](https://www.b2bits.com/trading_solutions/fix_log_analyzers)

## Quickstart

```
$ kubectl create namespace fixeye
$ kubectl create secret generic fixeye-license-file --from-file=path/to/fixeye-agent.license -n fixeye
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install my-fixeye b2bits/fixeye-agent -n fixeye
```

## Introduction

This chart bootstraps a [FixEye Agent](https://www.b2bits.com/trading_solutions/fix_log_analyzers) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.21+
- Helm 3.9.0+
- PV provisioner support in the underlying infrastructure
- Valid License Key

## Obtaining a License Key

To run FixEye Agent you will need a valid license key file. You can obtain a trial license from sales@btobits.com.

## Installing the Chart

To install the chart with the release name `my-fixeye`:

```
$ kubectl create namespace fixeye
$ kubectl create secret generic fixeye-license-file --from-file=path/to/fixeye-agent.license -n fixeye
$ helm repo add b2bits https://epam.github.io/b2bits-helmcharts
$ helm repo update
$ helm install my-fixeye b2bits/fixeye-agent -n fixeye
```

These commands deploy B2Bits FixEye Agent on the Kubernetes cluster in the default configuration. The **Parameters** section below lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-fixeye` deployment:

```
$ helm delete my-fixeye -n fixeye
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

| Parameter                                 | Description                                                      | Default                                                  |
| :---------------------------------------- | :--------------------------------------------------------------- | :------------------------------------------------------- |
| force_init_configs                        | Update config on re-deploy                                       | false                                                    |
| git_configs.url                           | Repository with configuration for fixeye                         | https://github.com/epam/b2bits-configuration-samples.git |
| git_configs.branch                        | Branch from the config repository                                | main                                                     |
| imagePullSecrets                          | The secret to downloading an image from a private repository     | []                                                       |
| fixeye.image.url                          | Repository with fixeye image                                     | b2bitsepam/fixeye-agent                                  |
| fixeye.image.version                      | Version of the fixeye image                                      | latest                                                   |
| fixeye.image.imagePullPolicy              | Image policy pull options                                        | Always                                                   |
| fixeye.port                               | Application port                                                 | 8882                                                     |
| fixeye.livenessProbe.initialDelaySeconds  | Number of seconds after the container has started before startup | 15                                                       |
| fixeye.livenessProbe.periodSeconds        | How often (in seconds) to perform the probe                      | 20                                                       |
| fixeye.resources                          | CPU/Memory resource requests/limits                              | Memory: 500Mi, CPU: 500m                                 |
| fixeye.storage.class                      | Storage class name                                               | gp2                                                      |
| fixeye.storage.accessModes                | Access Mode for storage class                                    | ReadWriteOnce                                            |
| fixeye.configs.size                       | Storage size for configs                                         | 1Gi                                                      |
| fixeye.samples.size                       | Storage size for samples                                         | 1Gi                                                      |
| fixeye.logs.size                          | Storage size for logs                                            | 10Gi                                                     |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install my-fixeye --set force_init_configs=true,fixeye.resources.limits.memory=800Mi b2bits/fixeye-agent -n fixeye
```

The above command forces pull configuration on start (`force_init_configs=true`) and sets RAM limit to `800Mi`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install my-fixeye -f values.yaml b2bits/fixeye-agent -n fixeye
```

> **Tip**: You can use the default [values.yaml](values.yaml) for reference.

## Configuration and installation details

### Using custom configuration

By default this helm chart clones FixEye Agent configuration from `https://github.com/epam/b2bits-configuration-samples/tree/main/fixeye-agent`.

If you choose to maintain your custom FixEye Agent configuration in private Git repository, you should provide SSH details for access to remote repository:

```
$ kubectl create secret generic ssh-creds --from-file=path/to/known_hosts --from-file=path/to/id_rsa -n fixeye
```

```
$ helm install my-fixeye --set git_configs.url=<your-git-repo-ssh-url>,git_configs.branch=<your-git-repo-branch> b2bits/fixeye-agent -n fixeye
```

It is expected that all FixEye Agent configuration files are grouped under `fixeye-agent` folder in your repository.

## Persistence

The [EPAM B2Bits FixEye Agent](https://hub.docker.com/r/b2bitsepam/fixeye-agent) image stores the FixEye Agent data at the following paths of the container:

- `/etc/fixeye` - Configuration files
- `/usr/share/doc/fixeye/samples` - Sample files
- `/var/log/fixeye` - Log files

Persistent Volume Claims are used to keep the data across deployments. This is known to work in AWS, minikube and k3s.
See the [Parameters](#parameters) section to configure the PVC.

## Where to get help

### Sales Information

Email: [sales@btobits.com](mailto:sales@b2bits.com)

### Technical Queries

For technical queries please contact [SupportFIXAntenna@epam.com](mailto:SupportFIXAntenna@epam.com)

## License

Copyright © B2BITS EPAM Systems Company 2000-2023

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
