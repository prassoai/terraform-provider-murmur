# Murmur Terraform Provider

The official Terraform provider for managing [Murmur](https://murmur.dev)
catalog resources.

> **Status:** Pre-release. The provider is not yet available from the public
> Terraform Registry. Published versions and documentation will appear in this
> repository when the first release is ready.

## Usage

```hcl
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

## Authentication

By default, the provider uses the first available credential in this order:

1. An `api_key` set in the provider block.
2. A `mur_`-prefixed service-profile API key from `MURMUR_API_KEY`.
3. The GitHub credential returned by `gh auth token`.

This makes an existing authenticated GitHub CLI session work for local
Terraform commands without additional configuration. Run `gh auth login` if
`gh auth token` does not return a credential.

For automation, set `auth = "api_key"` and provide `MURMUR_API_KEY`. To require
GitHub CLI authentication and disable API-key fallback, set `auth = "gh"`.
Avoid placing API keys directly in Terraform configuration or state.

## Support

Open an issue in this repository for provider bugs and feature requests.
