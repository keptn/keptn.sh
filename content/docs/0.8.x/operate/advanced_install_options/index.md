---
title: Advanced Install Options
description: Advanced Install Options using Helm
weight: 20
---

## Advanced Install Options: Install Keptn using the Helm chart

When executing `keptn install`, Keptn is installed via a Helm chart, which can also be used directly.
For this, the [Helm CLI](https://helm.sh) is required.

The command `helm upgrade ...` offers a flag called `--set`, which can be used to specify several configuration options using the format `key1=value1,key2=value2,...`.

The following flags are available:

<!-- generated using https://www.tablesgenerator.com/markdown_tables# -->
| Flag                                            | Description                                                                                                                                       | Possible Values                       | Default Value                  |
|-------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------|--------------------------------|
| `continuous-delivery.enabled`                         | Specifies whether the continuous-delivery execution-plane services (e.g., helm-service, jmeter-service) should be installed on the cluster or not | true, false                           | false                          |
| `continuous-delivery.<SERVICE-NAME>.image.repository` | Allows to specify an alternative repository for the docker image used                                                      | Container registry URL                | docker.io/keptn/<SERVICE-NAME> |
| `control-plane.apiGatewayNginx.type`                  | Specifies the Kubernetes service-type for api-gateway-nginx, which can be used to expose the Keptn API                               | NodePort, ClusterIP, LoadBalancer     | ClusterIP                      |
| `control-plane.prefixPath`                            | Allows to set a prefixPath for the api-gateway. This value has to start with a "/" but must not contain a trailing "/"                                                                                                    | URI, e.g., /mykeptn                 | empty                          |
| `control-plane.bridge.cliDownloadLink`                | Specifies a download link for the Keptn CLI which is displayed in Keptn Bridge                                                                   | any string/URL                        | null                           |
| `control-plane.bridge.versionCheck.enabled`           | Specifies whether the automatic version-check in Keptn Bridge should be enabled or not                                                           | true, false                           | true                           |
| `control-plane.bridge.showApiToken.enabled`           | Specifies whether the Keptn API Token and the `keptn auth` command should be shown in Keptn Bridge or not                                         | true, false                           | true                           |
| `control-plane.bridge.secret.enabled`                 | Specifies whether the secret used for the basic auth is enabled or not. Please be aware that if this value is set to `false`, there is no basic authentication for the bridge.                                         | true, false                           | true                           |
| `control-plane.bridge.installationType`               | Specifies which of the use cases are supported in the installation                                         | "QUALITY_GATES,CONTINUOUS_OPERATIONS", "QUALITY_GATES,CONTINUOUS_OPERATIONS,CONTINUOUS_DELIVERY"                           | "QUALITY_GATES,CONTINUOUS_OPERATIONS"                           |
| `control-plane.configurationService.storage`          | Allows to specify the storage-size of the persistent volume used in configuration-service                                                         | K8s volume storage size (e.g., 500Mi) | 100Mi                          |
| `control-plane.configurationService.storageClass`     | Allows to specify the storage-class of the persistent volume used in configuration-service                                                        | K8s storage class                     | null                           |
| `control-plane.<SERVICE-NAME>.image.repository`       | Allows to specify an alternative repository for the docker image                                                                                  | Container registry URL                | docker.io/keptn/<SERVICE-NAME> |
| `control-plane.mongodb.enabled`       | Allows to not install the default MongoDB instance in Keptn.                                                                                   | true,false               | true |
| `control-plane.mongodb.host`       | If an external MongoDB instance is used, specifiy the host name of this instance.                                                                                   | URL               | empty |
| `control-plane.mongodb.user`       | If an external MongoDB instance is used, specifiy the user to access the MongoDB.Keptn                                                                                   | any string               | empty |
| `control-plane.mongodb.password`       | If an external MongoDB instance is used, specifiy the password to access the MongoDB.                                                                                   | any string                | empty |
| `control-plane.mongodb.adminPassword`       | If an external MongoDB instance is used, specifiy the admin password to access the MongoDB                                                                                   | any string                | empty |

### Example: Use a LoadBalancer for api-gateway-nginx

```console
helm upgrade keptn keptn --install -n keptn --create-namespace --wait --version=0.8.4 --repo=https://storage.googleapis.com/keptn-installer --set=control-plane.apiGatewayNginx.type=LoadBalancer
```

### Example: Install execution plane for Continuous Delivery use-case

For example, the **Control Plane with the Execution Plane (for Continuous Delivery)** can be installed by the following command:
```console
helm upgrade keptn keptn --install -n keptn --create-namespace --wait --version=0.8.4 --repo=https://storage.googleapis.com/keptn-installer --set=continuous-delivery.enabled=true
```

### Example: Install execution plane for Continuous Delivery use-case and use a LoadBalancer for api-gateway-nginx

For example, the **Control Plane with the Execution Plane (for Continuous Delivery)** and a `LoadBalancer` for exposing Keptn can be installed by the following command:
```console
helm upgrade keptn keptn --install -n keptn --create-namespace --wait --version=0.8.4 --repo=https://storage.googleapis.com/keptn-installer --set=continuous-delivery.enabled=true,control-plane.apiGatewayNginx.type=LoadBalancer
```

### Example: Execute Helm upgrade without Internet connectivity

The following section contains instructions for installing Keptn in an air-gapped / offline installation scenario.

The following artifacts need to be available locally:

* Keptn Helm Chart (control-plane + helm + jmeter)
* Several Docker Images (e.g., pushed into a private registry)
* Helper scripts
* Keptn CLI (optional, but recommended for future use)

**Download Keptn Helm Charts**

Download the Helm charts from the [Keptn 0.8.4 release](https://github.com/keptn/keptn/releases/tag/0.8.4):

* Keptn Control Plane: https://github.com/keptn/keptn/releases/download/0.8.4/keptn-0.8.4.tgz
* helm-service (if needed): https://github.com/keptn/keptn/releases/download/0.8.4/helm-service-0.8.4.tgz
* jmeter-service (if needed): https://github.com/keptn/keptn/releases/download/0.8.4/jmeter-service-0.8.4.tgz

Move the helm charts to a directory on your local machine, e.g., `offline-keptn`.

For convenience, the following script creates this directory and downloads the required helm charts into it:

```console
mkdir offline-keptn
cd offline-keptn
curl -L https://github.com/keptn/keptn/releases/download/0.8.4/keptn-0.8.4.tgz -o keptn-0.8.4.tgz
curl -L https://github.com/keptn/keptn/releases/download/0.8.4/helm-service-0.8.4.tgz -o helm-service-0.8.4.tgz
curl -L https://github.com/keptn/keptn/releases/download/0.8.4/jmeter-service-0.8.4.tgz -o jmeter-service-0.8.4.tgz
cd ..
```

**Download Containers/Images**

Within the Helm Charts several Docker Images are referenced (Keptn specific and some third party dependencies).
We recommend to pulling, re-tagging and pushing those images to a local registry that the Kubernetes cluster can reach.

We are providing a helper script for this in our Git repository: https://github.com/keptn/keptn/blob/master/installer/airgapped/pull_and_retag_images.sh

For convenience, you can use the following commands to download and execute the script:

```console
cd offline-keptn
curl -L https://raw.githubusercontent.com/keptn/keptn/master/installer/airgapped/pull_and_retag_images.sh -o pull_and_retag_images.sh
chmod +x pull_and_retag_images.sh
KEPTN_TAG=0.8.4 ./pull_and_retag_images.sh "your-registry.localhost:5000/"
cd ..
```

Please mind the trailing slash for the registry url (e.g., `your-registry.localhost:5000/`).

**Installation**

Keptn's Helm chart allows you to specify the name of all images, which can be especially handy in air-gapped systems where you cannot access DockerHub for pulling the images.

We are providing a helper script for this in our Git repository: https://github.com/keptn/keptn/blob/master/installer/airgapped/install_keptn.sh

For convenience, you can use the following commands to download and execute the script:

```console
cd offline-keptn
curl -L https://raw.githubusercontent.com/keptn/keptn/master/installer/airgapped/install_keptn.sh -o install_keptn.sh
chmod +x install_keptn.sh
./install_keptn.sh "your-registry.localhost:5000/" keptn-0.8.4.tgz helm-service-0.8.4.tgz jmeter-service-0.8.4.tgz
cd ..
```

### Install Keptn using a Root-Context

The Helm chart allows customizing the root-context for the Keptn API and Bridge. 
By default, the Keptn API is located under `http://HOSTNAME/api` and the Keptn Bridge is located under `http://HOSTNAME/bridge`.
By specifying a value for `control-plane.prefixPath`, the used prefix for the root-context can be configured.
For example, if a user sets `control-plane.prefixPath=/mykeptn` in the Helm install/upgrade command,
the Keptn API is located under `http://HOSTNAME/mykeptn/api` and the Keptn Bridge is located under `http://HOSTNAME/mykeptn/bridge`:

```console
helm upgrade keptn keptn --install -n keptn --create-namespace --wait --version=0.8.4 --repo=https://storage.googleapis.com/keptn-installer --set=control-plane.apiGatewayNginx.type=LoadBalancer,continuous-delivery.enabled=true,control-plane.prefixPath=/mykeptn
```

### Example: Install Keptn with externally hosted MongoDB

If you want to use an externally hosted MongoDB instead of the MongoDB installed by Keptn, please use the `helm upgrade` command as shown below. Basically, provide the MongoDB host, user, and password as required for the connection string. 

```console
helm upgrade keptn keptn --install -n keptn --create-namespace 
--set=control-plane.mongodb.enabled=false,
      control-plane.mongodb.host=<YOUR_MONGODB_HOST>,
      control-plane.mongodb.user=<YOUR_MONGODB_USER>,
      control-plane.mongodb.password=<YOUR_MONGODB_PASSWORD>,
      control-plane.mongodb.adminPassword=<YOUR_MONGODB_ADMIN_PASSWORD>
```
