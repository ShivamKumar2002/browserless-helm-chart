# Browserless Helm Chart (Unofficial)

Welcome to the Browserless Helm chart (Unofficial) repository! This helm chart is designed to deploy the [Browserless](https://github.com/browserless/browserless) headless browser automation tool on a Kubernetes cluster. Browserless enables you to automate browser-based tasks without the need for a graphical user interface, providing a scalable and efficient way to perform web scraping, automated testing, and other web automation tasks.

## Overview

[Browserless](https://github.com/browserless/browserless) is a powerful tool for running headless instances of Chrome and Firefox in a containerized environment. It's perfect for tasks that require browser capabilities but do not need a visible UI, such as web scraping, PDF generation, and automated web testing.

This Helm chart simplifies the deployment of Browserless on Kubernetes, allowing you to easily configure and manage your Browserless instances at scale. Whether you're running a small cluster or managing a large-scale deployment, this chart provides the flexibility and control you need.

## Features

- **Easy Deployment**: Quickly deploy Browserless on your Kubernetes cluster with just a few commands.
- **Customizable Configuration**: Tailor Browserless settings to your needs with a wide range of configurable options.
- **Scalability**: Easily scale Browserless instances horizontally to handle increased workloads.
- **Health Checks**: Integrated health checks ensure that your Browserless instances are running smoothly.
- **Security**: Customizable security contexts and service accounts to align with your cluster's security policies.
- **Resource Management**: Define CPU and memory requests and limits to optimize resource utilization.

## Prerequisites

- A Kubernetes cluster (version 1.16 or higher)
- Helm 3.x installed on your local machine or CI/CD environment


## Quick Start

To get started with the Browserless Helm chart, follow these steps:

1. Add the Browserless Helm repository:

   ```bash
   helm repo add REPO_NAME REPO_URL
   helm repo update
   ```

2. Install the Browserless chart:

   ```bash
   helm install browserless REPO_NAME/browserless --namespace browserless --create-namespace --values my-values.yaml
   ```

   Replace `my-values.yaml` with the path to your custom values file if you have one.

3. Access Browserless through your Kubernetes service or Ingress, depending on your configuration.


## Configuration

The `values.yaml` file in the chart's directory contains the default configuration values. You can override these values by providing a custom `values.yaml` file or by setting individual parameters during the Helm installation or upgrade command.

For a detailed breakdown of the available configuration options, please refer to the [Values](#values) section of this README.

For detailed information on how to configure each aspect of the Browserless Helm chart, including service accounts, resource limits, ingress rules, and Browserless-specific settings, please see the [Values](#values) section of this README.

## Upgrading

To upgrade the Browserless deployment with new values or a new version of the chart, use the following Helm command:

```bash
helm upgrade browserless REPO_NAME/browserless --namespace browserless --values my-new-values.yaml
```

Ensure that `my-new-values.yaml` contains your updated configuration settings.


## Uninstallation

To remove the Browserless deployment from your Kubernetes cluster, execute the following command:

```bash
helm uninstall browserless --namespace browserless
```

This will delete all the resources associated with the Browserless Helm release.


## Values

The `values.yaml` file contains the default configuration values for the Browserless Helm chart. You can override these values by providing a custom `values.yaml` file or by setting individual parameters during the Helm installation or upgrade command.

### Basic Configuration

- `nameOverride`: Override the name of the chart.
- `fullnameOverride`: Override the full name of the chart.
- `clusterDomain`: The domain of the Kubernetes cluster (default: `cluster.local`).
- `replicaCount`: The number of Browserless pod replicas (default: `1`).
- `imagePullSecrets`: A list of secrets for authenticating with a Docker registry.

### Image Configuration

- `image.repository`: The Docker image repository for Browserless (default: `ghcr.io/browserless/chromium`).
- `image.tag`: The Docker image tag for Browserless (default: `latest`).
- `image.pullPolicy`: The image pull policy (default: `Always`).

### Service Account

- `serviceAccount.create`: Specifies whether a Kubernetes service account should be created for Browserless (default: `true`).
- `serviceAccount.automount`: Automatically mount the service account's API credentials (default: `true`).
- `serviceAccount.annotations`: Annotations to add to the service account.
- `serviceAccount.name`: The name of the service account to use.

### Browserless Configuration

The `config` section allows you to customize the Browserless application settings:

- `config.ALLOW_FILE_PROTOCOL`: Allow or deny file protocol requests (default: `false`).
- `config.CONCURRENT`: The maximum number of concurrent sessions (default: `5`).
- `config.CORS`: Enable or disable cross-origin-resource-sharing (default: `false`).
- `config.CORS_ALLOW_METHODS`: HTTP methods allowed when CORS is enabled.
- `config.CORS_ALLOW_ORIGIN`: Origins allowed when CORS is enabled.
- `config.HEALTH`: Enable health checks for CPU/Memory usage (default: `true`).
- `config.QUEUED`: The maximum number of items in the queue (default: `10`).
- `config.TIMEOUT`: The maximum time in milliseconds for any session to run (default: `120000`).
- `config.TOKEN`: A custom token for securing the Browserless instance (required).


### Service and Exposure

- `service.type`: The type of Kubernetes service to create (default: `ClusterIP`).
- `service.port`: The port on which the Browserless service will be exposed (default: `3000`).
- `ingress.enabled`: Enable or disable the creation of an Ingress resource (default: `false`).
- `ingress.className`: The Ingress class name to use.
- `ingress.annotations`: Annotations to add to the Ingress resource.
- `ingress.hosts`: A list of hosts to configure for the Ingress resource.
- `ingress.tls`: TLS configuration for the Ingress resource.

### Resources and Scaling

- `resources`: Resource requests and limits for the Browserless pods.
- `autoscaling`: Horizontal pod autoscaling configuration.


### Autoscaling (HPA)

The Helm chart provides configuration options for Horizontal Pod Autoscaling (HPA), which can be used to automatically scale the number of pods based on the observed CPU or memory utilization. Below are the details of the autoscaling configuration settings:

| Configuration Setting                   | Default Value | Description                                                                                                                                                                                                  |
|------------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `autoscaling.enabled`                   | `false`      | Enables or disables the Horizontal Pod Autoscaler for the deployment. When `true`, the autoscaler manages the scaling of your pods.                                                                                   |
| `autoscaling.minReplicas`               | `1`          | The minimum number of replicas that the autoscaler will maintain. This is the lower limit for scaling operations.                                                                                                      |
| `autoscaling.maxReplicas`               | `5`          | The maximum number of replicas to which the autoscaler can scale up. This is the upper limit for the number of pods that can be created.                                                                                     |
| `autoscaling.targetCPUUtilizationPercentage` | `80`         | The target CPU utilization percentage that the autoscaler aims to maintain. The autoscaler scales up/down based on whether the CPU usage exceeds or falls below this value.                                                      |
| `autoscaling.targetMemoryUtilizationPercentage` | Not set | The target memory utilization percentage for the autoscaler to consider. If set, the autoscaler will scale based on memory usage in addition to CPU usage. This setting is optional and will not take effect unless specified by the user. |


### Additional Configurations

- `additionalEnvVars`: A map of additional environment variables to set for the Browserless pods.
- `podAnnotations`: Annotations to add to the Browserless pods.
- `podLabels`: Labels to add to the Browserless pods.
- `podSecurityContext`: The security context for the pod.
- `securityContext`: The security context for the container running Browserless.


### Advanced Configuration

- `volumes`: Additional volumes to attach to the Browserless pods.
- `volumeMounts`: Additional volume mounts for the Browserless container.
- `nodeSelector`: Node labels for pod placement.
- `tolerations`: Tolerations for pod scheduling on tainted nodes.
- `affinity`: Affinity and anti-affinity rules for pod placement.


## Contributing

We welcome contributions to this chart! If you have a feature request, bug report, or improvement, please open an issue or submit a pull request.


## Support

If you encounter any issues or have questions about this, please open an issue in this repository, and I will be happy to assist you.


## Notes

**NOTE:** This chart is not affiliated with official browserless repo in any way.

## TODO

- [ ] Add REPO_NAME and REPO_URL.