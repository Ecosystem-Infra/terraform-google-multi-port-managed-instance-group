# Multi-port Managed Instance Group Terraform Module

Modular Google Compute Engine managed instance group for Terraform.

This repository is a fork, maintained in order to define a managed instance
group with seven service ports.

## Usage

```ruby
module "mig1" {
  source              = "GoogleCloudPlatform/managed-instance-group/google"
  version             = "1.1.14"
  region              = "${var.region}"
  zone                = "${var.zone}"
  name                = "group1"
  size                = 2
  service_port_1      = 80
  service_port_1_name = "http-primary"
  service_port_2      = 8000
  service_port_2_name = "http-secondary"
  service_port_3      = 443
  service_port_3_name = "https"
  service_port_4      = 8001
  service_port_4_name = "http2"
  service_port_5      = 8002
  service_port_5_name = "websocket"
  service_port_6      = 8003
  service_port_6_name = "websocket-secure"
  service_port_7      = 8004
  service_port_7_name = "https-secondary"
  https_health_check   = false
  target_pools        = ["${module.gce-lb-fr.target_pool}"]
  target_tags         = ["allow-service1"]
  ssh_source_ranges   = ["0.0.0.0/0"]
}
```

> NOTE: Make sure you are using [version pinning](https://www.terraform.io/docs/modules/usage.html#module-versions) to avoid unexpected changes when the module is updated.

## Resources created

- [`google_compute_instance_template.default`](https://www.terraform.io/docs/providers/google/r/compute_instance_template.html): The instance template assigned to the instance group.
- [`google_compute_instance_group_manager.default`](https://www.terraform.io/docs/providers/google/r/compute_instance_group_manager.html): The instange group manager that uses the instance template and target pools. 
- [`google_compute_firewall.default-ssh`](https://www.terraform.io/docs/providers/google/r/compute_firewall.html): Firewall rule to allow ssh access to the instances.
