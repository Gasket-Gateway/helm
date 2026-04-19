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
helm install gasket oci://ghcr.io/gasket-gateway/charts/gasket --version 0.1.7 -f values.yaml -n gasket
```

## Charts

| Chart                     | Description                | Version |
| ------------------------- | -------------------------- | ------- |
| [gasket](./charts/gasket) | Gasket Gateway (API/Portal) | 0.1.7   |

## Configuration

The chart renders `gasketConfig` as `config.yaml` and mounts it into the container. Key configuration sections:

| Section              | Description                                                     |
| -------------------- | --------------------------------------------------------------- |
| `server`             | Host, port, metrics port, debug mode                            |
| `oidc`               | OIDC provider settings (SSO)                                    |
| `database`           | PostgreSQL connection details                                   |
| `opensearch`         | OpenSearch connection for audit logging                         |
| `openai_backends`    | Config-defined OpenAI-compliant inference backends (read-only)  |
| `policies`           | Config-defined usage policies with versioned content (read-only)|
| `backend_profiles`   | Config-defined access profiles linking backends and policies     |
| `banners`            | Portal UI banners (info/warning, various positions)             |

### Policies

Policies define terms of use that users must accept before creating API keys. They can be pre-defined in `gasketConfig` or managed via the admin portal.

```yaml
gasketConfig: |
  policies:
    - name: "acceptable-use"
      description: "Standard acceptable use policy"
      content: |
        You must use this service responsibly and in accordance
        with organisational guidelines.

  backend_profiles:
    - name: "standard-tier"
      oidc_groups:
        - "gasket-users"
      backends:
        - "my-backend"
      policy_names:
        - "acceptable-use"
```

Config-defined policies are **read-only** in the admin portal. Admin-created policies support optional **reacceptance enforcement** — when enabled and the policy content changes, users must reaccept before creating new API keys.

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
3. Push the OCI chart to `ghcr.io/gasket-gateway/charts/<chart>`
