# Getting Started with Terraform

## Introduction
Terraform defines and provisions infrastructure as code (IaC). 
Write or configure infrastructure code (e.g., in Terraform, if applicable) that meets the design plan: correct modules, variables, resources, state handling, environment separation, etc.

## Installation instruction
To install Terraform, visit [Terraform.io](https://developer.hashicorp.com/terraform) and download the binary for your platform.

## Getting started
After installing Terraform, create your first infrastructure using a configuration file.

### Directory creation
Create a new directory on your local machine for Terraform configuration file(s).

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
  image = docker_image.nginx_image.name
  name  = "training"
  ports {
    internal = 80
    external = 80
  } 
}   
resource "docker_image" "nginx_image" {
  name = "nginx:latest"
}
```
### Initialize Terraform
Initialize Terraform. This command installs the required providers:

```shell
$ terraform init
Initializing the backend...
Initializing provider plugins...
- Reusing previous version of kreuzwerker/docker from the dependency lock file
- Using previously-installed kreuzwerker/docker v3.6.2

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

### Apply Configuration
Provision resources using:

```shell
$ terraform apply
Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  \# docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = "nginx:latest"
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "training"
      + network_data                                = (known after apply)
      + network_mode                                = "bridge"
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + healthcheck (known after apply)

      + labels (known after apply)

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  \# docker_image.nginx_image will be created
  + resource "docker_image" "nginx_image" {
      + id          = (known after apply)
      + image_id    = (known after apply)
      + name        = "nginx:latest"
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
  ```

  Enter a value: yes
```
docker_image.nginx_image: Creating...
docker_image.nginx_image: Still creating... [00m10s elapsed]
docker_image.nginx_image: Creation complete after 16s [id=sha256:029d4461bd98f124e531380505ceea2072418fdf28752aa73b7b273ba3048903nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=ffbea327e68eda96fcf47f8522946c75d1fb08d0fc2c969e6127433a76a3680f]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

Terraform takes a few minutes to run. It prints a message when it creates the resource(s).

### Destroy Infrastructure
Destroy the infrastructure when done:

```shell
$ terraform destroy 
docker_image.nginx_image: Refreshing state... [id=sha256:029d4461bd98f124e531380505ceea2072418fdf28752aa73b7b273ba3048903nginx:latest]
docker_container.nginx: Refreshing state... [id=ffbea327e68eda96fcf47f8522946c75d1fb08d0fc2c969e6127433a76a3680f]

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach                                      = false -> null
      - command                                     = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - container_read_refresh_timeout_milliseconds = 15000 -> null
      - cpu_shares                                  = 0 -> null
      - dns                                         = [] -> null
      - dns_opts                                    = [] -> null
      - dns_search                                  = [] -> null
      - entrypoint                                  = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env                                         = [] -> null
      - group_add                                   = [] -> null
      - hostname                                    = "ffbea327e68e" -> null
      - id                                          = "ffbea327e68eda96fcf47f8522946c75d1fb08d0fc2c969e6127433a76a3680f" -> null
      - image                                       = "nginx:latest" -> null
      - init                                        = false -> null
      - ipc_mode                                    = "private" -> null
      - log_driver                                  = "json-file" -> null
      - log_opts                                    = {} -> null
      - logs                                        = false -> null
      - max_retry_count                             = 0 -> null
      - memory                                      = 0 -> null
      - memory_swap                                 = 0 -> null
      - must_run                                    = true -> null
      - name                                        = "training" -> null
      - network_data                                = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.2"
              - ip_prefix_length          = 16
              - mac_address               = "4a:f4:50:5e:df:57"
              - network_name              = "bridge"
                # (2 unchanged attributes hidden)
            },
        ] -> null
      - network_mode                                = "bridge" -> null
      - privileged                                  = false -> null
      - publish_all_ports                           = false -> null
      - read_only                                   = false -> null
      - remove_volumes                              = true -> null
      - restart                                     = "no" -> null
      - rm                                          = false -> null
      - runtime                                     = "runc" -> null
      - security_opts                               = [] -> null
      - shm_size                                    = 64 -> null
      - start                                       = true -> null
      - stdin_open                                  = false -> null
      - stop_signal                                 = "SIGQUIT" -> null
      - stop_timeout                                = 1 -> null
      - storage_opts                                = {} -> null
      - sysctls                                     = {} -> null
      - tmpfs                                       = {} -> null
      - tty                                         = false -> null
      - wait                                        = false -> null
      - wait_timeout                                = 60 -> null
        # (7 unchanged attributes hidden)

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx_image will be destroyed
  - resource "docker_image" "nginx_image" {
      - id          = "sha256:029d4461bd98f124e531380505ceea2072418fdf28752aa73b7b273ba3048903nginx:latest" -> null
      - image_id    = "sha256:029d4461bd98f124e531380505ceea2072418fdf28752aa73b7b273ba3048903" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:029d4461bd98f124e531380505ceea2072418fdf28752aa73b7b273ba3048903" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: 
```
Enter a value: yes
```
docker_container.nginx: Destroying... [id=ffbea327e68eda96fcf47f8522946c75d1fb08d0fc2c969e6127433a76a3680f]
docker_container.nginx: Destruction complete after 1s
docker_image.nginx_image: Destroying... [id=sha256:029d4461bd98f124e531380505ceea2072418fdf28752aa73b7b273ba3048903nginx:latest]
docker_image.nginx_image: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.
```

Terraform removes all the resources it created earlier. Make sure to define dependencies between your resources so Terraform knows the correct order to destroy them. This helps prevent leftover or orphaned resources after cleanup.
