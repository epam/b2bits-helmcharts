# FIXEdge-CPP Helm Chart

### Overview
FIX server FIXEdge is an application server providing FIX connectivity to multiple clients. Client applications communicate with FIXEdge through one of the multiple transport protocols (e.g. Simple Sockets, JMS, IBM MQ) employing transport adaptors. It is designed to be as easy as possible to install, configure, administrate and monitor trading information flows. It is written in C++ and has a performance profile suitable for the needs of all clients up to and including large sell-side institutions and large volume traders. FIXEdge comes with a rich UI for monitoring session statuses and parameters in real-time.

### Get Repo Info

    helm repo add b2bits https://epam.github.io/b2bits-helmcharts
    helm repo update

*See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation.*

### Prepare
Create the necessary namespace, if necessary, you can use the existing one:

    kubectl create namespace fixedge

Use existing license or request a trial version from sales@btobits.com:

    kubectl create secret generic license-file --from-file=engine.license --namespace fixedge

If you want to use a private repository with configuration for fixedge, you need to add a key (by default, the configuration is in the public repository):

    kubectl create secret generic ssh-creds --from-file=known_hosts --from-file=id_rsa --namespace fixedge

*Command to generate known_hosts: `ssh-keyscan -H github.com > known_hosts` where `github.com` is the domain of your svc*

### Installing the Chart
To install the chart with the release name `my-release`:
    
    helm install my-release b2bits/fixedge --namespace fixedge

### Uninstalling the Chart
To uninstall/delete the my-release deployment:

    helm delete my-release --namespace fixedge

The command removes all the Kubernetes components associated with the chart and deletes the release.

### Configuration

#### FIXEdge:

| Parameter |  Description | Default |
| :-------- | :----------- | :------ |
| force_init_configs | Update config on re-deploy | false |
| git_configs.url | Repository with configuration for fixdge and fixicc-agent | https://github.com/epam/b2bits-configuration-samples.git |
| git_configs.branch | Branch from the config repository | main |
| imagePullSecrets | The secret to downloading an image from a private repository | [] |
| fixedge.image.url | Repository with fixedge image | b2bitsepam/fixedge-cpp |
| fixedge.image.version | Version of the fixedge image | latest |
| fixedge.image.imagePullPolicy | Image policy pull options | Always |
| fixedge.port | Application port | 8901 |
| fixedge.httpAdmPort | Admin application port  | 8903 |
| fixedge.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 15 |
| fixedge.livenessProbe.periodSeconds | How often (in seconds) to perform the probe | 20 |
| fixedge.resources | CPU/Memory resource requests/limits | Memory: 500Mi, CPU: 500m |
| fixedge.storage.class | Storage class name | gp2 |
| fixedge.storage.accessModes | Access Mode for storage class | ReadWriteOnce |
| fixedge.fe_configs.size | Storage size | 1Gi |
| fixedge.fe_sessions_logs.size | Storage size | 1Gi |
| fixedge.fe_app_logs.size| Storage size | 1Gi |

Run with parameter override:

    helm install fixedge b2bits/fixedge-cpp --namespace fixedge\
    --set fixedge.resources.requests.cpu=1 \
    --set fixedge.resources.requests.memory=1Gi \
    --set fixedge.resources.limits.cpu=1 \
    --set fixedge.resources.limits.memory=1Gi

# FIXEdge-JAVA Helm Chart

### Overview
FIXEdge® Java (FEJ) is an application server providing FIX connectivity. Orders routing to multiple destinations, receiving and distributing executions to internal systems, fanning out quotes and receiving market data, capturing and reporting trades are just few use case examples covered by FEJ. Unique internal architecture allows supporting balance between flexibility and high-performance requirements.

### Get Repo Info

    helm repo add b2bits https://epam.github.io/b2bits-helmcharts
    helm repo update

*See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation.*

### Prepare
Create the necessary namespace, if necessary, you can use the existing one:

    kubectl create namespace fixedge-java

Use existing license or request a trial version from sales@btobits.com:

    kubectl create secret generic fixaj2-java-license-file --from-file=path/to/fixaj2-license.bin --namespace fixedge-java
    kubectl create secret generic fixedge-java-license-file --from-file=path/to/fixedgej-license.bin --namespace fixedge-java

If you want to use a private repository with configuration for fixedge-java, you need to add a key (by default, the configuration is in the public repository):

    kubectl create secret generic ssh-creds --from-file=known_hosts --from-file=id_rsa --namespace fixedge-java

*Command to generate known_hosts: `ssh-keyscan -H github.com > known_hosts` where `github.com` is the domain of your svc*

### Installing the Chart
To install the chart with the release name `my-release`:
    
    helm install my-release b2bits/fixedge-java --namespace fixedge-java

### Uninstalling the Chart
To uninstall/delete the my-release deployment:

    helm delete my-release --namespace fixedge-java

The command removes all the Kubernetes components associated with the chart and deletes the release.

### Configuration

#### FIXEdge Java:

| Parameter |  Description | Default |
| :-------- | :----------- | :------ |
| force_init_configs | Update config on re-deploy | false |
| git_configs.url | Repository with configuration for fixdge and fixicc-agent | https://github.com/epam/b2bits-configuration-samples.git |
| git_configs.branch | Branch from the config repository | main |
| imagePullSecrets | The secret to downloading an image from a private repository | [] |
| fixedge_java.image.url | Repository with fixedge image | b2bitsepam/fixedge-java |
| fixedge_java.image.version | Version of the fixedge image | latest |
| fixedge_java.image.imagePullPolicy | Image policy pull options | Always |
| fixedge_java.port | Application port | 8911 |
| fixedge_java.httpAdmPort | Admin application port  | 9010 |
| fixedge_java.livenessProbe.initialDelaySeconds | Number of seconds after the container has started before startup | 15 |
| fixedge_java.livenessProbe.periodSeconds | How often (in seconds) to perform the probe | 20 |
| fixedge_java.resources | CPU/Memory resource requests/limits | Memory: 500Mi, CPU: 500m |
| fixedge_java.storage.class | Storage class name | gp2 |
| fixedge_java.storage.accessModes | Access Mode for storage class | ReadWriteOnce |
| fixedge_java.fe_configs.size | Storage size | 1Gi |
| fixedge_java.fe_sessions_logs.size | Storage size | 1Gi |
| fixedge_java.fe_app_logs.size| Storage size | 1Gi |

Run with parameter override:

    helm install fixedge-java b2bits/fixedge-java --namespace fixedge-java \
    --set fixedge_java.resources.requests.cpu=1 \
    --set fixedge_java.resources.requests.memory=1Gi \
    --set fixedge_java.resources.limits.cpu=1 \
    --set fixedge_java.resources.limits.memory=1Gi

# FIXICC-H2 Helm Chart

### Overview
FIXICC H2 is a new generation FIX Integrated Control Center application available via a web browser. It is designed to administer, configure and monitor the FIXEDGE line of products, including standalone FIX engines and clusters of FIX engines..

### Get Repo Info

    helm repo add b2bits https://epam.github.io/b2bits-helmcharts
    helm repo update

*See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation.*

### Prepare
Create the necessary namespace, if necessary, you can use the existing one:

    kubectl create namespace fixicc-h2

If you want to use a private repository with configuration for fixicc-h2, you need to add a key (by default, the configuration is in the public repository):

    kubectl create secret generic ssh-creds --from-file=known_hosts --from-file=id_rsa --namespace fixicc-h2

*Command to generate known_hosts: `ssh-keyscan -H github.com > known_hosts` where `github.com` is the domain of your svc*

### Installing the Chart
To install the chart with the release name `my-release`:
    
    helm install my-release b2bits/fixicc-h2 --namespace fixicc-h2

### Uninstalling the Chart
To uninstall/delete the my-release deployment:

    helm delete my-release --namespace fixicc-h2

The command removes all the Kubernetes components associated with the chart and deletes the release.

### Configuration

#### FIXICC-H2:

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

Run with parameter override:

    helm install fixicc-h2 b2bits/fixicc-h2 --namespace fixicc-h2 \
    --set fixicc.resources.requests.cpu=2 \
    --set fixicc.resources.requests.memory=2Gi \
    --set fixicc.resources.limits.cpu=2 \
    --set fixicc.resources.limits.memory=2Gi
