<div align="center">

# 📦 Hytale Helm Chart

[![Origin: Forgejo](https://img.shields.io/badge/Forgejo-Origin-orange?style=for-the-badge&logo=forgejo&logoSize=auto)](https://forgejo.sakul-flee.de/HelmCharts/Hytale/)
[![Mirror: GitHub](https://img.shields.io/badge/GitHub-Mirror-blue?style=for-the-badge&logo=github&logoSize=auto)](https://github.com/SakulFlee/HytaleChart/)

[![Based on: HytaleContainer](https://img.shields.io/badge/Based%20On-Hytale%20Container-blue?style=for-the-badge&logo=docker&logoSize=auto)](https://forgejo.sakul-flee.de/Containers/Hytale/)

A feature rich Helm Chart for running Hytale servers!

</div>

## Features

- Authentication token caching
- Automatic updating upon restart
- [Planned] Mod downloading

## Running

Install the Helm Chart:

```bash
helm install hytale oci://ghcr.io/sakulflee/charts/hytale
```

Then following instructions in Helm!  
Most setups you won't need to change the values.

### Values

#### General

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| name | string | `"hytale"` | Name of container |
| labels | object | `{}` |  |

#### Persistence

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| persistence.size | string | `"10Gi"` |  |
| persistence.storageClass | string | `""` |  |

#### Resources 

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| resources.limits.cpu | string | `"4000m"` |  |
| resources.limits.memory | string | `"8Gi"` |  |
| resources.requests.cpu | string | `"2000m"` |  |
| resources.requests.memory | string | `"4Gi"` |  |

> [!NOTE]
> Increasing the memory here doesn't automatically give the Hytale server more!
> Check JVM Arguments (`-Xmx`) for further adjustments.

#### Service

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| service.port | int | `5520` |  |
| service.type | string | `"LoadBalancer"` |  |
| service.udpPort | int | `520` |  |

#### Credentials

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| credentials.credentialsKey | string | `"credentials.json"` | Key inside secret |
| credentials.secretName | string | `"hytale-credentials"` | Name of secret (can be empty; see below!) |

> [!NOTE]
> This is _optional_, you can set the `secretName` to an empty string and use the interactive authentication instead.

#### JVM Arguments

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| jvm.additionalOptions[0] | string | `"-server"` |  |
| jvm.additionalOptions[1] | string | `"-XX:+UseG1GC"` |  |
| jvm.additionalOptions[2] | string | `"-XX:+UseStringDeduplication"` |  |
| jvm.additionalOptions[3] | string | `"-Xmx4G"` | Max Java memory usage, adjust with requests & limits! |
| jvm.additionalOptions[4] | string | `"-Xms512M"` |  |
| jvm.additionalOptions[5] | string | `"-server"` |  |

> [!NOTE]
> This is a map that can be overwritten.

#### Image

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ghcr.io/sakulflee/hytale"` |  |
| image.tag | string | `"latest"` |  |

#### Probes

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| probes.liveness.initialDelaySeconds | int | `30` |  |
| probes.liveness.periodSeconds | int | `30` |  |
| probes.startup.failureThreshold | int | `100` |  |
| probes.startup.file | string | `"/tmp/.ready"` |  |
| probes.startup.periodSeconds | int | `30` |  |

#### Security

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| securityContext.fsGroup | int | `1000` |  |
| securityContext.runAsGroup | int | `1000` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `1000` |  |

## License

This project is licensed under [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html).
