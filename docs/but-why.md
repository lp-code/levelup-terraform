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

![But why](https://raw.githubusercontent.com/jpedro/levelup-terraform/master/.github/assets/images/why.jpg)

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

- More coverage of AWS resource than CFN itself
- Support for multi-region and multi-account deployments
- Doing things that in AWS requires custom lambda code (for example,
  accepting a Transit Gateway attachments)


### GCP

GCP gave up its own Deployment Manager (superior to CFN) in favour of
Terraform official modules.

### Azure

For Azure, Microsoft has created its own IaC solution, Bicep, which
[looks a lot like TF](https://learn.microsoft.com/en-us/training/modules/introduction-to-infrastructure-as-code-using-bicep/4-what-bicep).
However, they aim to give you good support whatever your favourite IaC tool is.
For example, in 2022, Microsoft released
[Microsoft Azure Export for Terraform](https://github.com/Azure/aztfexport),
a tool to bring your existing Azure resources under the management of Terraform.
There is also support for combining TF with ARM templates or Bicep, or direct
Azure API access, for example to create resource types that are in preview.
