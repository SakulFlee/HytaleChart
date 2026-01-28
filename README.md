<div align="center">

# 📦 Hytale Helm Chart


![](https://img.shields.io/badge/Forgejo-Origin-orange?style=for-the-badge&logo=forgejo&logoSize=auto&link=https%3A%2F%2Fforgejo.sakul-flee.de%2FHelmCharts%2FHytale%2F)
![](https://img.shields.io/badge/GitHub-Mirror-blue?style=for-the-badge&logo=github&logoSize=auto&link=https%3A%2F%2Fgithub.com%2FSakulFlee%2FHytaleCharts)

![](https://img.shields.io/badge/Based%20on-Hytale%20Container-blue?style=for-the-badge&logo=docker&link=https%3A%2F%2Fforgejo.sakul-flee.de%2FContainers%2FHytale%2F)

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

### Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| credentials.credentialsKey | string | `"credentials.json"` | Key inside secret |
| credentials.secretName | string | `"hytale-credentials"` | Name of secret (can be empty if you wish interactive setup!) |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ghcr.io/sakulflee/hytale"` |  |
| image.tag | string | `"latest"` |  |
| jvm.additionalOptions[0] | string | `"-server"` |  |
| jvm.additionalOptions[1] | string | `"-XX:+UseG1GC"` |  |
| jvm.additionalOptions[2] | string | `"-XX:+UseStringDeduplication"` |  |
| jvm.additionalOptions[3] | string | `"-Xmx4G"` |  |
| jvm.additionalOptions[4] | string | `"-Xms512M"` |  |
| jvm.additionalOptions[5] | string | `"-server"` |  |
| labels | object | `{}` |  |
| name | string | `"hytale"` |  |
| persistence.size | string | `"10Gi"` |  |
| persistence.storageClass | string | `""` |  |
| probes.liveness.initialDelaySeconds | int | `30` |  |
| probes.liveness.periodSeconds | int | `30` |  |
| probes.startup.failureThreshold | int | `100` |  |
| probes.startup.file | string | `"/tmp/.ready"` |  |
| probes.startup.periodSeconds | int | `30` |  |
| resources.limits.cpu | string | `"4000m"` |  |
| resources.limits.memory | string | `"8Gi"` |  |
| resources.requests.cpu | string | `"2000m"` |  |
| resources.requests.memory | string | `"4Gi"` |  |
| securityContext.fsGroup | int | `1000` |  |
| securityContext.runAsGroup | int | `1000` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| securityContext.runAsUser | int | `1000` |  |
| service.port | int | `5520` |  |
| service.type | string | `"LoadBalancer"` |  |
| service.udpPort | int | `520` |  |

## License

This project is licensed under [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html).
