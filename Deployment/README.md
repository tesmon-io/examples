## Test Run Engine (TRE) deployment and usage

This repository contains sample scripts for deploying the TRE in various environments and example test cases to get started.

### Local deployment

TRE is available as a docker container. To run TRE locally, execute the following commands:

```shell
docker pull ghcr.io/tesmon-sys/tesmon-tre:latest
docker run -e TESMON_TRE_ID=<TEST_RUN_ENGINE_ID>  -e TESMON_API_TOKEN=<API_TOKEN> ghcr.io/tesmon-sys/tesmon-tre:latest
```

Parameters reference:

TEST_RUN_ENGINE_ID = TRE ID found in environments page - [Environments](https://app.tesmon.io/environments)

API_TOKEN = Token found in your profile/settings page - [Token](https://app.tesmon.io/settings/tokens)

#### Updating local TRE to the latest version
Repeat both the docker pull and docker run commands as shown above.

### Deploying to AWS

Prerequisites:
- Terraform: Follow instructions to [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli).

TRE uses Elastic Container Service and [Terraform](https://www.terraform.io/) for deployments in AWS.

> **_NOTE:_** Deploying TRE inside your AWS VPC & security group provides TRE access to all the resources in your environment so you can access them in your tests.

1. Edit **env_vars_agent_core.json** file
Update TEST_RUN_ENGINE_ID and API_TOKEN as explained above.

2. Edit **terraform.tfvars** file
Update the following variables with the values from your environment:
```
customer_vpc_id = "vpc-***fix-me***"
customer_subnet_id = "subnet-***fix-me***"
customer_security_group_id = "***fix-me***"
```

3. Deploy by running Terraform
```
terraform init
terraform apply
```

When prompted, review and type 'yes' to continue deployment

It would create an elastic container service cluster with the name "tesmonprod-production-cluster" and deploy the test run engine as a container task. The name of the cluster is derived from app_name and app_environment values in terraform.tfvars

#### Updating TRE to the latest version in ECS

* Delete the existing ECS service in ECS (not the cluster itself)
* Wait for some time - typically 30 seconds for the service to be deleted.
* Update your terraform files with the above content
* Run terraform deployment commands again


