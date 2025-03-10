---
subcategory: "EC2 (Elastic Compute Cloud)"
layout: "aws"
page_title: "aws_ami"
description: |-
  Get information on an Amazon Machine Image (AMI).
---

[describe-images]: https://docs.cloud.croc.ru/en/api/ec2/images/DescribeImages.html

# Data Source: aws_ami

Use this data source to get the ID of a registered image for use in other resources.

## Example Usage

```terraform
data "aws_ami" "example" {
  executable_users = ["self"]
  most_recent      = true
  name_regex       = "^example\\d{1}"
  owners           = ["self"]

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}
```

## Argument Reference

* `owners` - (Required) List of image owners to limit search. At least one value must be specified.
  Valid items are the project ID (`project@customer`) or `self`.
* `most_recent` - (Optional) If more than one result is returned, use the most recent image.
* `executable_users` - (Optional) Limit search to project with *explicit* launch permission on the image.
  Valid items are the project ID (`project@customer`), `all` or `self`.

* `filter` - (Optional) One or more name/value pairs to filter.

For more information about filtering, see the [EC2 API documentation][describe-images].

* `name_regex` - (Optional) A regex string to apply to the image list returned by the EC2 API.
  This allows more advanced filtering. It is done locally on what the EC2 API returns,
  and could have a performance impact if the result is large.
  It is recommended to combine this with other options to narrow down the list the EC2 API returns.

~> **Note** If more or less than a single match is returned by the search,
Terraform will fail. Ensure that your search is specific enough to return
a single image ID only, or use `most_recent` to choose the most recent one. If
you want to match multiple images, use the [`aws_ami_ids`](ami_ids.md) data source instead.

## Attributes Reference

### Supported attributes

`id` is set to the ID of the found image.

In addition, the following attributes are exported:

~> **Note** Some values are not always set and may not be available for
interpolation.

* `arn` - The ARN of the image.
* `architecture` - The OS architecture of the image (ie: `i386` or `x86_64`).
* `block_device_mappings` - Set of objects with block device mappings of the image.
    * `device_name` - The physical name of the device.
    * `ebs` - Map containing EBS information, if the device is EBS based. Unlike most object attributes, these are accessed directly (e.g., `ebs.volume_size` or `ebs["volume_size"]`) rather than accessed through the first element of a list (e.g., `ebs[0].volume_size`).
        * `delete_on_termination` - `true` if the EBS volume will be deleted on termination.
        * `iops` - `0` if the EBS volume is not a provisioned IOPS image, otherwise the supported IOPS count.
        * `snapshot_id` - The ID of the snapshot.
        * `volume_size` - The size of the volume, in GiB.
        * `volume_type` - The volume type.
    * `virtual_name` - The virtual device name (for instance stores).
* `description` - The description of the image that was provided during image
  creation.
* `image_id` - The ID of the image. Should be the same as the resource `id`.
* `image_owner_alias` -  The alias of the image owner name.
* `image_type` - The type of image.
* `name` - The name of the image that was provided during image creation.
* `owner_id` - The project ID.
* `platform` - The value is Windows for `Windows` images; otherwise blank.
* `public` - `true` if the image has public launch permissions.
* `root_device_name` - The device name of the root device.
* `root_device_type` - The type of root device (ie: `ebs` or `instance-store`).
* `root_snapshot_id` - The snapshot id associated with the root device, if any
  (only applies to `ebs` root devices).
* `state` - The current state of the image. If the state is `available`, the image
  is successfully registered and can be used to launch an instance.
* `tags` - Any tags assigned to the image.
* `virtualization_type` - The type of virtualization of the image (ie: `hvm`).

### Unsupported attributes

~> **Note** These attributes may be present in the `terraform.tfstate` file but they have preset values and cannot be specified in configuration files.

The following attributes are not currently supported:

`boot_mode`, `creation_date`, `deprecation_time`, `ena_support`, `hypervisor`, `image_location`, `kernel_id`, `platform_details`, `product_codes`, `ramdisk_id`, `state_reason`, `sriov_net_support`, `usage_operation`.
