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
  auth   = "api_key"
}
```

Set `MURMUR_API_KEY` to a `mur_`-prefixed service-profile API key. Avoid
placing API keys directly in Terraform configuration or state.

## Support

Open an issue in this repository for provider bugs and feature requests.
