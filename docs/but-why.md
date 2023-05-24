# What

Terraform is an Infrastructure-as-Code (IaC) tool.

More precisely, it's an API aggregator that manages the lifecycle of
resources.

IaC means you can commit, review, automate and audit changes to your
infrastructure.

Other benefits of Iac are:

- idempotency: run and re-run 1000 times always creates the same results
- orchestration: do this *then* that
- modularity: this brings extensibility and reusability (don't reinvent
  the wheel)


## Why

![But why](../.github/assets/images/why.jpg)

Terraform was created in 2014 because Hashicorp realised the power of
CloudFormation and they wanted that but not for AWS only.


### Adoption

Now Terraform is the most used IaC tool in the world. It has more than
3000 providers.

Commonly used for cloud resources but any API that exposes CRUD
endpoints can be wrapped in a provider and its lifecycle managed with
terraform.

https://www.hashicorp.com/resources/creating-terraform-provider-for-anything

Examples:

- Active Directory objects
- GitHub repos, users and teams
- Uptimerobots alerts
- Cloudflare DNS records
- Kubernetes resources


### AWS

Terraform beats AWS' CloudFormation at its own game by having:

- more coverage of AWS resource than CFN itself
- support for multi-region and multi-account deployments
- doing things that in AWS requires custom lambda code (for example,
  accepting a Transit Gateway attachment)


### GCP

GCP gave up its own Deployment Manager (superior to CFN) in favour of
Terraform official modules.

### Azure

Azure favour its Bicep, which
[looks a lot like TF](https://learn.microsoft.com/en-us/training/modules/introduction-to-infrastructure-as-code-using-bicep/4-what-bicep).
