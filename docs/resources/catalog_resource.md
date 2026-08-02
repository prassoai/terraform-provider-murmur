---
page_title: "murmur_catalog_resource Resource - Murmur"
description: |-
  Manages one resource in a Murmur tenant catalog.
---

# murmur_catalog_resource

Manages one named Murmur catalog resource. The Murmur API remains authoritative
for which catalog kinds exist and for each kind's payload validation.

The provider rejects secret-bearing catalog kinds because Terraform state is
not an encrypted secret store.

## Example Usage

```terraform
resource "murmur_catalog_resource" "reviewer" {
  kind     = "agent-persona"
  name     = "reviewer"
  markdown = file("${path.module}/reviewer.md")
}
```

JSON-backed resources use `payload` instead. Configure exactly one of
`payload` or `markdown`.

## Schema

### Required

- `kind` (String) Catalog kind. Changing it replaces the Terraform resource.
- `name` (String) Catalog resource name. Changing it replaces the Terraform
  resource. A top-level `name` in `payload` must match this value.

### Optional

- `tenant` (String) Assertion that this resource belongs to the configured
  provider tenant. The provider always sends the tenant from the provider
  block and refuses a mismatch before making an API request.
- `payload` (String) Desired JSON document. Compared semantically rather than
  byte-for-byte.
- `markdown` (String) Markdown with YAML frontmatter for markdown-backed kinds.
- `ignore_fields` (Set of String) Top-level fields owned by another Murmur
  workflow. Changes to these fields are not reported as drift and Terraform
  preserves their live values during writes. Cannot be combined with
  `markdown`.

### Read-Only

- `generation` (String) Server-assigned version of the stored resource. It
  changes on every write.

## Workspace Image Ownership

Murmur image rollouts update a workspace's `imageRef`. Terraform-managed
workspaces should therefore ignore that field:

```terraform
resource "murmur_catalog_resource" "workspace" {
  kind     = "workspace"
  name     = "example"
  payload  = file("${path.module}/workspace.json")

  ignore_fields = ["imageRef"]
}
```

The payload must still provide the initial `imageRef` when creating the
workspace. After creation, Terraform preserves the live value when other
workspace fields change and does not report image rollouts as drift.

## Import

Import an existing resource with `<kind>/<name>`:

```shell
terraform import murmur_catalog_resource.example placement/example
```

The provider tenant must be configured before import. Import reads the live
resource and records it as Terraform's drift baseline without modifying it.

## Concurrent Changes

Writes and deletes are conditional on the version Terraform previously read or
wrote. If another actor changes the resource, Terraform reports the conflict
instead of overwriting or deleting that newer version. Run `terraform plan` to
review and reconcile the change.
