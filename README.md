# webapp-helm-chart

## Overview

This Helm chart deploys a web server and configures a Bitnami PostgreSQL database with Flyway migration for managing database schema updates. The chart uses Kubernetes ConfigMaps and Secrets to store configuration data and sensitive information securely.

### Prerequisites

- A running Kubernetes cluster
- Helm installed and configured on your local machine
- A Bitnami PostgreSQL container registry or a Docker image repository
### Helm Chart Configuration

To deploy this Helm chart, you need to configure the following parameters:

- **webServer**:
  - `enabled`: Set to `true` to deploy the web server.
  - `image.repository`: The Docker image repository for the web server.
  - `image.tag`: The Docker image tag for the web server.
  - `service.port`: The port on which the web server will be exposed.
  - `configMap.webServerConfig`: The name of the ConfigMap containing web server configuration.
- **postgresql**:
  - `enabled`: Set to `true` to deploy the Bitnami PostgreSQL database.
  - `image.registry`: The container registry for the Bitnami PostgreSQL image.
  - `image.repository`: The Docker image repository for the Bitnami PostgreSQL image.
  - `image.tag`: The Docker image tag for the Bitnami PostgreSQL image.
  - `rootPassword.secret`: The name of the Secret containing the root password for the PostgreSQL database.
  - `db.name`: The name of the database.
  - `db.user`: The username for the database.
  - `db.password.secret`: The name of the Secret containing the database user password.

### Installing the Helm Chart

Use the following command to install the Helm chart with your custom configuration values:

```shell
helm install my-release -f values.yaml .
```

- `my-release` should be replaced with a unique name for your release.
- `values.yaml` should contain your custom configuration. You can use the sample `values.yaml` provided in the Helm chart repository as a starting point.

### Flyway Migration

The Helm chart automates database schema updates using Flyway migration. Place your Flyway SQL scripts in the `flyway/sql` directory within your chart.

### ConfigMap and Secrets

You can customize the configuration and store sensitive data in ConfigMaps and Secrets. Make sure to set the appropriate values in your `values.yaml` file to reference the ConfigMap and Secret names.

### Uninstalling the Helm Chart

To uninstall the Helm chart, use the following command:

```shell
helm uninstall release-name
```

- `my-release` should be the name of the release you specified during installation.
testt-123--1-2


