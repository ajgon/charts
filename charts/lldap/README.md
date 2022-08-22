<!--- app-name: lldap -->

# Helm chart for LLDAP

LLDAP is a lightweight authentication server that provides an opinionated, simplified LDAP interface for authentication.

[Overview of lldap](https://github.com/nitnelave/lldap/)

## TL;DR

```bash
helm repo add ajgon https://charts.rzegocki.pl/
helm install my-release ajgon/lldap
```

## Introduction

This chart bootstraps a [lldap](https://hub.docker.com/r/nitnelave/lldap) deployment on a
[Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install my-release ajgon/lldap
```

The command deploys LLDAP on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section
lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Common parameters

| Name                         | Description                                          | Value           |
| ---------------------------- | ---------------------------------------------------- | --------------- |
| `domain`                     | Basic domain, which will be exposed                  | `""`            |
| `nameOverride`               | String to partially override common.names.fullname   | `""`            |
| `fullnameOverride`           | String to fully override common.names.fullname       | `""`            |
| `affinity`                   | Affinity for pods assignment                         | `{}`            |
| `clusterDomain`              | Kubernetes cluster domain name                       | `cluster.local` |
| `commonAnnotations`          | Annotations to add to all deployed objects           | `{}`            |
| `commonLabels`               | Labels to add to all deployed objects                | `{}`            |
| `extraDeploy`                | Array of extra objects to deploy with the release    | `[]`            |
| `imagePullSecrets`           | Docker registry secret names as an array             | `[]`            |
| `nodeSelector`               | Node labels for pods assignment                      | `{}`            |
| `podAnnotations`             | Annotations for pods                                 | `{}`            |
| `replicaCount`               | Number of replicas                                   | `""`            |
| `resources`                  | Limit resources for the conainers                    | `{}`            |
| `secretAnnotations`          | Annotations to add to secret                         | `{}`            |
| `securityContext`            | Run containers as a specific securityContext         | `{}`            |
| `tolerations`                | Tolerations for pods assignment                      | `[]`            |
| `serviceAccount.create`      | Specifies whether a ServiceAccount should be created | `true`          |
| `serviceAccount.name`        | The name of the ServiceAccount to use                | `""`            |
| `serviceAccount.annotations` | Additional custom annotations for the ServiceAccount | `{}`            |
| `service.type`               | Kubernetes service type for traffic                  | `ClusterIP`     |
| `service.httpPort`           | Port for http traffic                                | `17170`         |
| `service.ldapPort`           | Port for ldap traffic                                | `389`           |
| `service.ldapsPort`          | Port for ldaps traffic (if ldaps enabled)            | `636`           |

### Image parameters

| Name                | Description                                      | Value             |
| ------------------- | ------------------------------------------------ | ----------------- |
| `image.registry`    | lldap image registry                             | `docker.io`       |
| `image.repository`  | lldap image repository                           | `nitnelave/lldap` |
| `image.tag`         | lldap image tag (immutable tags are recommended) | `v0.4.0`          |
| `image.pullPolicy`  | lldap image pull policy                          | `""`              |
| `image.pullSecrets` | lldap image pull secrets                         | `[]`              |
| `image.debug`       | Enable image debug mode                          | `false`           |

### Ingress configuration

| Name                                 | Description                                        | Value    |
| ------------------------------------ | -------------------------------------------------- | -------- |
| `ingress.enabled`                    | Enable ingress                                     | `false`  |
| `ingress.className`                  | Add ingress class name                             | `""`     |
| `ingress.annotations`                | Add ingress annotations                            | `{}`     |
| `ingress.hosts[0].host`              | Add host for ingress, if empty domain will be used | `""`     |
| `ingress.hosts[0].paths[0].path`     | Add path for each ingress host                     | `/`      |
| `ingress.hosts[0].paths[0].pathType` | Add ingress path type                              | `Prefix` |
| `ingress.tls`                        | Add ingress tls settings                           | `[]`     |

### Persistence configuration

| Name                        | Description                                                    | Value               |
| --------------------------- | -------------------------------------------------------------- | ------------------- |
| `persistence.enabled`       | Enable lldap data persistence using PVC                        | `false`             |
| `persistence.medium`        | Provide a medium for emptyDir volumes                          | `""`                |
| `persistence.storageClass`  | Persistent Volume storage class                                | `""`                |
| `persistence.accessModes`   | Persistent Volume access modes                                 | `["ReadWriteOnce"]` |
| `persistence.size`          | Persistent Volume size                                         | `32Mi`              |
| `persistence.selector`      | Additional labels to match for the PVC                         | `{}`                |
| `persistence.dataSource`    | Custom PVC data source                                         | `{}`                |
| `persistence.existingClaim` | Use a existing PVC which must be created manually before bound | `""`                |

### Configuration parameters

| Name                       | Description                                                           | Value             |
| -------------------------- | --------------------------------------------------------------------- | ----------------- |
| `log.verbose`              | Tune the logging to be more verbose by setting this to be true        | `false`           |
| `jwt.secret`               | Random secret for JWT signature                                       | `""`              |
| `jwt.useSecretFile`        | Mount jwt secret as file instead of using an environment variable     | `true`            |
| `ldap.baseDn`              | Base DN for LDAP                                                      | `""`              |
| `ldap.userDn`              | Admin username                                                        | `admin`           |
| `ldap.userEmail`           | Admin email                                                           | `admin@localhost` |
| `ldap.userPass`            | Admin password                                                        | `""`              |
| `ldap.useSecretFile`       | Mount password as file instead of using an environment variable       | `true`            |
| `keyFile.value`            | Private key - 128 bytes encoded in base64                             | `""`              |
| `ignoredAttributes.user`   | Ignored user attributes                                               | `[]`              |
| `ignoredAttributes.group`  | Ignored group attributes                                              | `[]`              |
| `smtp.enablePasswordReset` | Whether to enabled password reset via email, from LLDAP               | `false`           |
| `smtp.server`              | The SMTP server                                                       | `""`              |
| `smtp.port`                | The SMTP port                                                         | `587`             |
| `smtp.encryption`          | How the connection is encrypted, either "TLS" or "STARTTLS"           | `STARTTLS`        |
| `smtp.from`                | The header field, optional: how the sender appears in the email       | `""`              |
| `smtp.replyTo`             | The header field, optional: who should receive a reply of lldap email | `""`              |
| `smtp.user`                | The SMTP user, usually your email address                             | `""`              |
| `smtp.password`            | The SMTP password                                                     | `""`              |
| `smtp.useSecretFile`       | Mount password as file instead of using an environment variable       | `true`            |
| `ldaps.enabled`            | Whether to enable LDAPS                                               | `false`           |
| `ldaps.certificateKey`     | Private key in PEM#8 format                                           | `""`              |
| `ldaps.certificate`        | Certificate                                                           | `""`              |
