---
subcategory: "IAM (Identity & Access Management)"
layout: "aws"
page_title: "aws_iam_users"
description: |-
  Provides ARNs and names of selected IAM users.
---

# Data Source: aws_iam_users

Provides ARNs (Amazon Resource Names) and the names of selected IAM users.

## Example Usage

### All Users

```terraform
data "aws_iam_users" "selected" {}
```

### Users Filtered By Name Regex

```terraform
data "aws_iam_users" "selected" {
  name_regex = "user.*"
}
```

## Argument Reference

The following arguments are supported:

* `name_regex` - (Optional) A regex string to apply to the list of users returned by IAM API.

~> **Note** This filtering is performed locally and can affect performance when the list of users is large.

## Attribute Reference

### Supported attributes

* `arns` - List of ARNs of the users.
* `id` - The region (e.g., `region-1`).
* `names` - List of user names.

### Unsupported attributes

~> **Note** These attributes may be present in the `terraform.tfstate` file but they have preset values and cannot be specified in configuration files.

The following attributes are not currently supported: `path_prefix`.
