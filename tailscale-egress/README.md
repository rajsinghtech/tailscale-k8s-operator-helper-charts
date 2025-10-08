# Tailscale Egress

Helm chart for generating Tailscale egress service manifests.

## Installation

```bash
helm install tailscale-egress oci://ghcr.io/rajsinghtech/tailscale-k8s-operator-helper-charts/tailscale-egress --version 0.0.0-latest
```

## Configuration

### Proxy Configuration

Choose one of the following (mutually exclusive):

- `proxyGroup`: Tailscale proxy group name
- `proxyClass`: Tailscale proxy class name

If neither is set, services will be created without proxy annotations.

### Tags

Tags can be set when using `proxyClass` or when neither proxy option is set. Cannot be used with `proxyGroup`.

```yaml
tags:
  - "tag:example"
  - "tag:production"
```

### Services

Each service requires:
- `name`: Service name
- `fqdn` OR `ip`: Tailscale FQDN or IP address
- `ports`: List of ports (required when `proxyGroup` is set)

```yaml
services:
  - name: example-service
    fqdn: example.tailnet.ts.net
    ports:
      - port: 443
        protocol: TCP
```

## Examples

### With Proxy Group

```yaml
proxyGroup: common-egress

services:
  - name: api-service
    fqdn: api.tailnet.ts.net
    ports:
      - port: 443
        protocol: TCP
```

### With Proxy Class and Tags

```yaml
proxyClass: generic
tags:
  - "tag:production"

services:
  - name: web-service
    ip: 10.0.0.1
    ports:
      - port: 80
        protocol: TCP
```

### Without Proxy Configuration

```yaml
services:
  - name: internal-service
    fqdn: internal.tailnet.ts.net
```

## Validation Rules

- Cannot set both `proxyGroup` and `proxyClass`
- Cannot set both `proxyGroup` and `tags`
- Services must define `ports` when `proxyGroup` is set
