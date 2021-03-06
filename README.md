# VPC peered with a Private Space

This example provisions an AWS VPC via the [mars/heroku_aws_vpc](https://github.com/mars/terraform-aws-vpc) module, a new Private Space, peers them automatically, and deploys a Heroku app and an AWS ECS app that form a two-way (duplex) health check.

![Diagram of example duplex health check via private IP addresses across the peering connection](doc/terraform-aws-vpc-peered-v01.png)

## Requirements

An [AWS IAM](https://console.aws.amazon.com/iam/home) user (`aws_access_key` & `aws_secret_key` in Usage below).

Name suggestion: `terraform-vpc-peered-health-provisioner`

With policies:
* **AmazonEC2FullAccess**
* **AmazonECS_FullAccess**
* **AmazonVPCFullAccess**
* **IAMFullAccess**
* **CloudWatchLogsFullAccess**

## Usage

```bash
export \
  TF_VAR_heroku_email=name@example.com \
  TF_VAR_heroku_enterprise_team=xxxxx \
  TF_VAR_heroku_api_key=xxxxx \
  TF_VAR_aws_access_key=xxxxx \
  TF_VAR_aws_secret_key=xxxxx \
  TF_VAR_instance_public_key='ssh-rsa xxxxx…' 

terraform apply \
  -var name=my-deployment-name \
  -var aws_region=us-west-2
```

Once apply completes successfully, visit the output `health_public_url` in a web browser.

⏲ *It may take a few minutes for all services to start & DNS records to propagate.*

Eventually, the Health Check web UI should reflect a healthy duplex connection:

![Screenshot of a good Health Check](doc/health-check-ok.png)
