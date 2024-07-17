# Guide to Helm Charts

## What is Helm?

Helm is a package manager for Kubernetes, facilitating the deployment and management of applications within a Kubernetes cluster. Helm helps define, install, and upgrade complex Kubernetes applications by packaging them into charts.

## Key Concepts

- **Chart**: A Helm package that contains all the resource definitions needed to run an application on a Kubernetes cluster.
- **Release**: An instance of a chart running in a Kubernetes cluster.
- **Repository**: A collection of charts available for download and use.

## Use Cases for Helm Charts

1. **Simplified Deployment**: Helm charts provide a straightforward way to deploy applications with complex configurations.
2. **Version Control**: Charts can be versioned, making it easy to manage and rollback changes.
3. **Sharing Applications**: Charts can be shared with others, enabling consistent deployments across different environments.
4. **Reusable Templates**: Charts allow for the reuse of Kubernetes manifests with templating.

## Getting Started with Helm Charts

### Installation

1. **Install Helm**: Follow the [official Helm installation guide](https://helm.sh/docs/intro/install/).
2. **Add a Chart Repository**:
   ```bash
   helm repo add stable https://charts.helm.sh/stable
   helm repo update
   ```

### Creating a Helm Chart

1. **Create a New Chart**:
   ```bash
   helm create mychart
   ```
   This command creates a new directory `mychart` with a basic directory structure.

2. **Directory Structure**:
   ```
   mychart/
   ├── charts/
   ├── templates/
   │   ├── deployment.yaml
   │   ├── _helpers.tpl
   │   ├── service.yaml
   │   └── ingress.yaml
   ├── Chart.yaml
   ├── values.yaml
   └── README.md
   ```

### Installing a Helm Chart

To install a chart, use the following command:
```bash
helm install myrelease mychart
```

This will install the chart `mychart` with the release name `myrelease`.

### Upgrading a Helm Chart

To upgrade an existing release, use:
```bash
helm upgrade myrelease mychart
```

### Uninstalling a Helm Chart

To uninstall a release, use:
```bash
helm uninstall myrelease
```

### Example Helm Chart

Here is an example `values.yaml` for a simple Nginx deployment:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.19.6"

service:
  type: LoadBalancer
  port: 80

ingress:
  enabled: true
  annotations: {}
  hosts:
    - host: chart-example.local
      paths: ["/"]
  tls: []
```

## Important Related Terms and Topics

- **Kubernetes**: The container orchestration platform that Helm works with.
- **Kubernetes Manifests**: The YAML files used to describe the desired state of resources in a Kubernetes cluster.
- **Templating**: Using Go templates in Helm charts to dynamically generate Kubernetes manifests.
- **Helm Repositories**: Online collections of charts available for download and use.
- **Values Files**: YAML files used to override default values in Helm charts.

## References and Additional Learning

- [Helm Official Documentation](https://helm.sh/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Awesome Helm](https://github.com/cdwv/awesome-helm)
- [Artifact Hub](https://artifacthub.io/)

## Conclusion

Helm charts are an invaluable tool for managing Kubernetes applications. By packaging applications into charts, Helm simplifies deployment, version control, and sharing of Kubernetes applications. This guide provided an introduction to Helm charts, their use cases, and examples to get started. For more detailed information, explore the references and additional learning links provided.

# Zero to Hero with Helm

## Introduction

Helm is the package manager for Kubernetes, and it makes deploying complex applications simple and repeatable. This guide will take you from a beginner to a proficient Helm user, covering everything from installation to advanced usage.

## Table of Contents

1. [Installation](#installation)
2. [Helm Basics](#helm-basics)
3. [Creating Your First Helm Chart](#creating-your-first-helm-chart)
4. [Deploying Applications with Helm](#deploying-applications-with-helm)
5. [Managing Helm Releases](#managing-helm-releases)
6. [Templating with Helm](#templating-with-helm)
7. [Using Helm Repositories](#using-helm-repositories)
8. [Advanced Helm Usage](#advanced-helm-usage)
9. [Best Practices](#best-practices)
10. [Additional Resources](#additional-resources)

## Installation

### Installing Helm

1. **Download and Install Helm**:
   - **macOS**:
     ```bash
     brew install helm
     ```
   - **Windows**:
     ```bash
     choco install kubernetes-helm
     ```
   - **Linux**:
     ```bash
     curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
     ```

2. **Verify Installation**:
   ```bash
   helm version
   ```

### Installing Kubernetes (Minikube)

1. **Download and Install Minikube**:
   - Follow the instructions for your OS from the [official Minikube installation guide](https://minikube.sigs.k8s.io/docs/start/).

2. **Start Minikube**:
   ```bash
   minikube start
   ```

## Helm Basics

### Understanding Helm Components

- **Helm CLI**: The command-line interface for Helm.
- **Chart**: A package of pre-configured Kubernetes resources.
- **Release**: A running instance of a chart.
- **Repository**: A collection of Helm charts.

### Basic Helm Commands

- **Install a Chart**:
  ```bash
  helm install <release-name> <chart-name>
  ```
- **List Releases**:
  ```bash
  helm list
  ```
- **Upgrade a Release**:
  ```bash
  helm upgrade <release-name> <chart-name>
  ```
- **Uninstall a Release**:
  ```bash
  helm uninstall <release-name>
  ```

## Creating Your First Helm Chart

### Creating a New Chart

1. **Create a Chart**:
   ```bash
   helm create mychart
   ```
2. **Understand the Chart Structure**:
   ```
   mychart/
   ├── charts/
   ├── templates/
   │   ├── deployment.yaml
   │   ├── _helpers.tpl
   │   ├── service.yaml
   │   └── ingress.yaml
   ├── Chart.yaml
   ├── values.yaml
   └── README.md
   ```

### Customizing the Chart

1. **Edit `values.yaml`**:
   ```yaml
   replicaCount: 2
   image:
     repository: nginx
     tag: "1.19.6"
   service:
     type: LoadBalancer
     port: 80
   ingress:
     enabled: true
     annotations: {}
     hosts:
       - host: chart-example.local
         paths: ["/"]
     tls: []
   ```

2. **Install the Customized Chart**:
   ```bash
   helm install myrelease ./mychart
   ```

## Deploying Applications with Helm

### Example: Deploying Nginx

1. **Add the Bitnami Repository**:
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm repo update
   ```

2. **Install Nginx**:
   ```bash
   helm install my-nginx bitnami/nginx
   ```

### Viewing the Deployment

- **Check Pods**:
  ```bash
  kubectl get pods
  ```

- **Check Services**:
  ```bash
  kubectl get svc
  ```

## Managing Helm Releases

### Upgrading a Release

1. **Modify `values.yaml`**:
   ```yaml
   replicaCount: 3
   ```

2. **Upgrade the Release**:
   ```bash
   helm upgrade myrelease ./mychart
   ```

### Rolling Back a Release

1. **Rollback**:
   ```bash
   helm rollback myrelease 1
   ```

## Templating with Helm

### Using Templates

1. **Edit `deployment.yaml`**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: {{ .Release.Name }}-nginx
     labels:
       app: {{ .Release.Name }}
   spec:
     replicas: {{ .Values.replicaCount }}
     selector:
       matchLabels:
         app: {{ .Release.Name }}
     template:
       metadata:
         labels:
           app: {{ .Release.Name }}
       spec:
         containers:
           - name: nginx
             image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
             ports:
               - containerPort: 80
   ```

2. **Render Templates Locally**:
   ```bash
   helm template mychart
   ```

## Using Helm Repositories

### Adding and Updating Repositories

1. **Add a Repository**:
   ```bash
   helm repo add stable https://charts.helm.sh/stable
   ```

2. **Update Repositories**:
   ```bash
   helm repo update
   ```

3. **Search for Charts**:
   ```bash
   helm search repo nginx
   ```

## Advanced Helm Usage

### Using Subcharts

1. **Add Subchart**:
   - Place the subchart in the `charts/` directory of the main chart.

2. **Dependencies in `Chart.yaml`**:
   ```yaml
   dependencies:
   - name: subchart
     version: 1.0.0
     repository: "file://../subchart"
   ```

3. **Update Dependencies**:
   ```bash
   helm dependency update
   ```

### Helm Hooks

1. **Define a Hook**:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: "{{ .Release.Name }}-test"
     annotations:
       "helm.sh/hook": test-success
   spec:
     containers:
       - name: test
         image: busybox
         command: ['sh', '-c', 'echo The test is successful!']
   ```

2. **Install with Hooks**:
   ```bash
   helm install myrelease ./mychart
   ```

## Best Practices

1. **Use `values.yaml` for Configuration**: Keep your configuration in `values.yaml` to make it easier to manage.
2. **Version Control**: Use a version control system for your Helm charts.
3. **Modularize Charts**: Break down complex charts into subcharts for better maintainability.
4. **Documentation**: Document your charts and values for easy understanding and usage.

## Additional Resources

- [Helm Official Documentation](https://helm.sh/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Helm Charts Repository](https://artifacthub.io/)
- [Awesome Helm](https://github.com/cdwv/awesome-helm)
- [Helm Hub](https://hub.helm.sh/)

## Conclusion

Helm is a powerful tool that simplifies the deployment and management of applications on Kubernetes. By following this "Zero to Hero" guide, you will gain a solid understanding of Helm and be able to leverage its full potential to manage your Kubernetes applications effectively.
