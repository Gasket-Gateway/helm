# helm

Helm charts for deploying **Gasket Gateway**.

## Add the Helm Repository

```bash
helm repo add gasket-gateway https://Gasket-Gateway.github.io/helm
helm repo update
```

## Install

```bash
helm install gasket gasket-gateway/gasket
```

## OCI (alternative)

Charts are also published to the GitHub Container Registry:

```bash
helm install gasket oci://ghcr.io/gasket-gateway/gasket --version 0.3.0
```

## Charts

| Chart                     | Description                | Version |
| ------------------------- | -------------------------- | ------- |
| [gasket](./charts/gasket) | Gasket Gateway application | 0.3.0   |

## Development

```bash
# Lint
helm lint charts/gasket

# Package
helm package charts/gasket

# Template (dry-run)
helm template gasket charts/gasket
```

## Release Process

Releases are automated via GitHub Actions ([`.github/workflows/release.yml`](.github/workflows/release.yml)).
Pushing a chart version bump to `main` will:

1. Create a GitHub Release with the packaged `.tgz`
2. Update the `gh-pages` branch `index.yaml` (classic Helm repo)
3. Push the OCI chart to `ghcr.io/gasket-gateway/<chart>`
