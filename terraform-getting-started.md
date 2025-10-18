# Getting Started with Terraform

## Introduction
Terraform defines and provisions infrastructure as code (IaC). 

## Installation instruction
To install Terraform, visit [Terraform.io](https://developer.hashicorp.com/terraform) and download the binary for your platform.

## Getting started
After installing Terraform, create your first infrastructure using a configuration file.

### Directory creation
Create a new directory on your local machine for Terraform configuration files.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

### Creating configuration file
Create a Terraform configuration file:

```shell
$ touch main.tf
```

### Adding the code to file
Paste the following lines into ```main.tf``` 

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```
### Initialize Terraform
Initialize Terraform. This command installs the required providers:

```shell
$ terraform init
```

### Apply Configuration
Check for errors after initialization. Provision resources using:

```shell
$ terraform apply
```

Terraform takes a few minutes to run. It prints a message when it creates the resource.
### Destroy Infrastructure
Destroy the infrastructure when done:

```shell
$ terraform destroy
```
Type yes to confirm. Terraform removes all resources created previously.
Look for a message are the bottom of the output asking for confirmation. Type `yes` and hit ENTER. Terraform will destroy the resources it had created earlier.
