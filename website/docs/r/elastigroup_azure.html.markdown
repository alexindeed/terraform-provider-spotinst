---
layout: "spotinst"
page_title: "Spotinst: elastigroup_azure"
sidebar_current: "docs-do-resource-elastigroup_azure"
description: |-
 Provides a Spotinst elastigroup resource for Microsoft Azure.
---

# spotinst\_elastigroup\_azure

Provides a Spotinst elastigroup Azure resource.

## Example Usage

```hcl
resource "elastigroup_azure" "test_azure_group" {
  name                = "example_elastigroup_azure"
  resource_group_name = "spotinst-azure"
  region              = "eastus"
  product             = "Linux"

  user_data = ""

  // --- CAPACITY ------------------------------------------------------
  min_size         = 0
  max_size         = 1
  desired_capacity = 1
  // -------------------------------------------------------------------

  // --- INSTANCE TYPES ------------------------------------------------
  od_sizes           = ["standard_a1_v1", "standard_a1_v2"]
  low_priority_sizes = ["standard_a1_v1", "standard_a1_v2"]
  // -------------------------------------------------------------------

  // --- IMAGE ---------------------------------------------------------
  image = {
    // market image
    publisher = "OpenLogic"
    offer     = "CentOS"
    sku       = "7.3"
  }
  // -------------------------------------------------------------------

  // --- STRATEGY ------------------------------------------------------
  strategy = {
    od_count                = 1
    draining_timeout        = 300
  }
  // -------------------------------------------------------------------

  // --- LOAD BALANCERS ------------------------------------------------
  load_balancers = [{
    type          = "MULTAI_TARGET_SET"
    balancer_id   = "lb-1ee2e3q"
    target_set_id = "ts-3eq"
    auto_weight   = true
  }]

  // -------------------------------------------------------------------

  // --- HEALTH-CHECKS -------------------------------------------------
  health_check = {
    health_check_type = "INSTANCE_STATE"
    grace_period      = 120
    auto_healing      = true
  }
  // -------------------------------------------------------------------

  // --- NETWORK -------------------------------------------------------
  network = {
    virtual_network_name = "vname"
    subnet_name          = "my-subnet-name"
    resource_group_name  = "subnetResourceGroup"
    assign_public_ip     = true
  }
  // -------------------------------------------------------------------

  // --- LOGIN ---------------------------------------------------------
  login = {
    user_name      = "admin"
    ssh_public_key = "33a2s1f3g5a1df5g1ad3f2g1adfg56dfg=="
  }
  // -------------------------------------------------------------------
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The group name.
* `region` - (Required) The region your Azure group will be created in.
* `resource_group_name` - (Required) Name of the Resource Group for Elastigroup.
* `product` - (Required) Operation system type. Valid values: `"Linux"`, `"Windows"`.
* `max_size` - (Required) The maximum number of instances the group should have at any time.
* `min_size` - (Required) The minimum number of instances the group should have at any time.
* `desired_capacity` - (Required) The desired number of instances the group should have at any time.

* `od_sizes` - (Required) Available On-Demand sizes
* `low_priority_sizes` - (Required) Available Low-Priority sizes.

* `strategy` - (Required) Describes the deployment strategy.
* `low_priority_percentage` - (Optional, Default `100`) Percentage of Low Priority instances to maintain. Required if `od_count` is not specified.
* `od_count` - (Optional) Number of On-Demand instances to maintain. Required if low_priority_percentage is not specified.
* `draining_timeout` - (Optional, Default `120`) Time (seconds) to allow the instance to be drained from incoming TCP connections and detached from MLB before terminating it during a scale-down operation.

* `load_balancers` - (Required) Describes a set of one or more classic load balancer target groups and/or Multai load balancer target sets.
* `type` - (Required) The resource type. Valid values: CLASSIC, TARGET_GROUP, MULTAI_TARGET_SET.
* `balancer_id` - (Required) The balancer ID.
* `target_set_id` - (Required) The scale set ID associated with the load balancer.
* `auto_weight` - (Optional, Default: `false`)

* `image` - (Required) Image of a VM. An image is a template for creating new VMs. Choose from Azure image catalogue (marketplace) or use a custom image.
* `publisher` - (Optional) Image publisher. Required if resource_group_name is not specified.
* `offer` - (Optional) Name of the image to use. Required if publisher is specified.
* `user_data` - (Optional) Base64-encoded MIME user data to make available to the instances.
* `sku` - (Optional) Image’s Stock Keeping Unit, which is the specific version of the image. Required if publisher is specified.
* `resource_group_name` - (Optional) Name of Resource Group for custom image. Required if publisher not specified.
* `image_name` - (Optional) Name of the custom image. Required if resource_group_name is specified.

* `health_check` - (Optional) Describes the health check configuration.
* `health_check_type` - (Optional) Health check used to validate VM health. Valid values: “INSTANCE_STATE”.
* `grace_period` - (Optional) Period of time (seconds) to wait for VM to reach healthiness before monitoring for unhealthiness.
* `auto_healing` - (Optional) Enable auto-healing of unhealthy VMs.

* `network` - (Required) Defines the Virtual Network and Subnet for your Elastigroup.
* `virtual_network_name` - (Required) Name of Vnet.
* `subnet_name` - (Required) ID of subnet.
* `resource_group_name` - (Required) Vnet Resource Group Name.
* `assign_public_up` - (Optional, Default: `false`) Assign a public IP to each VM in the Elastigroup.

* `login` - (Required) Describes the login configuration.
* `user_name` - (Required) Set admin access for accessing your VMs.
* `ssh_public_key` - (Optional) SSH for admin access to Linux VMs. Required for Linux product types.
* `password` - (Optional) Password for admin access to Windows VMs. Required for Windows product types.

