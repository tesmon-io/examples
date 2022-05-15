# TRE (Test Run Engine) deployment and usage

This repository contains sample scripts for deploying the TRE in various environments and example test cases to get started.

## Deploying locally

TRE is available as a docker container. To run TRE locally, execute the following command:

```shell
docker run -e TESMON_AGENT_ID=valid_agent_id  -e TESMON_API_TOKEN="valid_api_token" ghcr.io/tesmon-sys/tesmon-tre:latest
```

Here,
valid_agent_id = Agent ID found in environments page - https://app.tesmon.io/environments

valid_api_token = Token found in your profile / settings page - https://app.tesmon.io/settings/tokens

## Deploying to Elastic Container Service

We have sample terraform scripts inside the terraform directory in this repository to deploy TRE inside your AWS environment.

Deploying directly inside your AWS vpc & security group provides TRE access to all the resources in your environment so you can execute tests againts them.

Replace the following data in the terraform directory:
**env_vars_agent_core.json**
Replace with valid agent id and api token as discussed for local deployment

**terraform.tfvars file:**
Fix the following variables to have the correct value so they have access to the resources that are tested:
```
customer_vpc_id = "vpc-***fix-me***"
customer_subnet_id = "subnet-***fix-me***"
customer_security_group_id = "***fix-me***"
```
Once terraform is installed, cd into the terraform directory and execute the following commands:

```
terraform init
terraform apply
```

When prompted, review and type 'yes' to continue deployment

It would create an elastic container service cluster with the name "tesmonprod-production-cluster" and deploy the test run engine as a container task. The name of the cluster is derived from app_name and app_environment values in terraform.tfvars

# Upgrading test run engine to the latest version in ECS

* Delete the existing ECS service in ECS (not the cluster itself)
* Wait for some time - typically 30 seconds for the service to be deleted.
* Update your terraform files with the above content
* Run terraform deployment commands again


