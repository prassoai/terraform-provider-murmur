---
page_title: "Murmur Provider"
description: |-
  The Murmur provider manages tenant catalog resources through the Murmur API.
---

# Murmur Provider

The Murmur provider manages resources in a single Murmur tenant. It uses the
same catalog API as the Murmur CLI and applies conditional writes so concurrent
changes are reported instead of silently overwritten.

## Example Usage

```terraform
terraform {
  required_providers {
    murmur = {
      source  = "prassoai/murmur"
      version = "~> 0.1"
    }
  }
}

provider "murmur" {
  tenant = "github_app/example-org"
}
```

For local use, the default authentication mode checks for an API key and then
uses the credential returned by `gh auth token`. For automation, set
`MURMUR_API_KEY` to a `mur_`-prefixed service-profile API key and set
`auth = "api_key"`.

## Schema

### Optional

- `tenant` (String) Tenant namespace in `{provider}/{org}` form, such as
  `github_app/example-org`. It is required when the configuration manages a
  Murmur resource.
- `api_address` (String) Murmur gRPC endpoint. Defaults to
  `api.murmur.dev:9090`. TLS is always enabled.
- `auth` (String) Authentication mode: `auto`, `gh`, or `api_key`. The default
  is `auto`.
- `api_key` (String, Sensitive) Inline `mur_`-prefixed service-profile API key.
  Prefer `MURMUR_API_KEY` so credentials do not appear in configuration.
- `ca_cert` (String) Base64-encoded PEM CA certificate to trust in addition to
  system roots.

## Authentication

`auto` uses the first available credential in this order:

1. The provider block's `api_key`.
2. `MURMUR_API_KEY`.
3. `gh auth token`.

`gh` uses only `gh auth token`. `api_key` uses only the inline key or
`MURMUR_API_KEY`; it never falls back to GitHub authentication.
