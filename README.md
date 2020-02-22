# Hasura on AWS

![supports terraform-0.12][terraform-0.12-badge]

Terraform module to deploy [Hasura GraphQL Engine](https://github.com/hasura/graphql-engine) on AWS. This will deploy
across multiple Availability Zones (AZ) with the following components:

- Postgres RDS deployed in multiple AZ
- Hasura GraphQL Engine in [Fargate](https://aws.amazon.com/fargate/) across multiple AZ
- ALB for load balancing between the hasura tasks
- Certificate issued by ACM for securing traffic to ALB
- Logging for RDS, ECS, and ALB into Cloudwatch Logs

## Support

The master branch supports Terraform v0.12 and later. For Terraform v0.11 and older versions [check other git branches](https://github.com/Rayraegah/hasura-aws/tree/terraform-0.11).

## Glossary

- AZ: Availability Zone
- ALB: Application Load Balancer
- ACM: AWS Certificate Manager
- Bastion: A Bastion host to allow migrations on RDS
- ECS: Elastic Container Service
- RDS: Relational Database Service

## Requirements

- AWS account
  - IAM user
  - domain with Route53
- Terraform v0.12

## Usage

Copy and paste into your Terraform configuration, edit the variables, and run `terraform init`

```terraform
module "hasura" {
  source                    = "Rayraegah/hasura/aws"
  version                   = "2.X.Y"
  region                    = "YOUR DEPLOYMENT REGION"
  domain                    = "YOUR DOMAIN NAME"
  app_subdomain             = "YOUR HASURA APP SUBDOMAIN"
  hasura_version_tag        = "HASURA VERSION TAG FOR DEPLOYMENT"
  hasura_admin_secret       = "YOUR HASURA ADMIN SECRET"
  hasura_jwt_secret_algo    = "ALGORITHM FOR JWT VERIFICATION (HMAC or RS256)"
  hasura_jwt_secret_key     = "YOUR PUBLIC KEY FOR JWT VERIFICATION"
  rds_db_name               = "YOUR DATABASE NAME"
  rds_instance              = "YOUR DATABASE INSTANCE SIZE"
  rds_username              = "YOUR DATABASE USERNAME"
  rds_password              = "YOUR DATABASE PASSWORD"
  multi_az                  = "ENABLE MULTIPLE AVAILABILITY ZONES"
  az_count                  = "NUMBER OF AVAILABILITY ZONES"
  vpc_enable_dns_hostnames  = "ENABLE DNS HOSTNAMES"
  environment               = "ENV VARS FOR ECS TASK"
  allowed_bastion_ips       = "YOUR INTERNAL NETWORKS IP ADDRESS"
  migrator_public_key       = "THE PUBLIC KEY OF YOUR MIGRATOR"
}
```

## License

Released under MIT License. Based on [Gordon Johnston](https://github.com/elgordino)'s proposed architecture.

[terraform-0.12-badge]: https://img.shields.io/badge/terraform-0.12-brightgreen.svg
