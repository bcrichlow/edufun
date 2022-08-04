# Getting Started with Terraform

Terraform is the most popular language for defining and provisioning infrastructure as code (IaC).

## Objectives
In this guide, you will learn to:
- Create Terraform configuration file
- Initialize and apply a Terraform plan
- Destroy Terraform infastruture
## Prerequisites
- Review: [What is Terraform?](https://www.terraform.io/intro)
- [Terraform version 1.1 or later](https://www.terraform.io/downloads.html)
- [Minikube](https://minikube.sigs.k8s.io)
- [Docker](https://https://www.docker.com/)
  


## Install Terraform
To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html). Select the appropriate package for your system and download the zip archive.

With Terraform installed, let's create some infrastructure.

## Terraform Workflow
The Terraform workflow is write, plan, and apply. In this guide, you will perform all three stages. See [What is Terraform?](https://www.terraform.io/intro) for more details.

## Select Location
Create a new directory for the Terraform configuration code.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```
## Configure Terraform 
Next, create your Terraform configuration file. This file defines the resources Terraform deploys.

```shell
$ touch main.tf
```

Paste the following into the configuration file.

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

resource "docker_image" "nginx" {
  name = "nginx:latest"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 8000
  }
}

```
## Initialize
Initialize Terraform with the `init` command. The docker provider will install.

```shell
$ terraform init
```

## Apply
Check for any errors. Then provision the resource with the `apply` command. 

```shell
$ terraform apply
```

The `terraform apply` command may take a few minutes to complete. When the plan is applied, Terraform will display: `Apply complete!`. The `apply` command creates a new plan.

## Destroy Resources
Finally, destroy the infrastructure.

```shell
$ terraform destroy
```

Look for the confirmation message at the bottom of the output. Type `yes` to confirm and press ENTER. 
```shell
Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes
```

Terraform will destroy the resources created earlier.


## Next steps

In this guide, you learned how to:
- Create Terraform configuration file
- Initialize Terraform
- Apply a Terraform plan
- Destroy Terraform-created resources

Continue learning more with our [Terraform Getting Started Docs.](https://www.terraform.io/docs#get-started)