# Workflows

The most typical worflow for Terraform is:

```bash
terraform init
terraform plan
terraform apply
```

In reverse order:


## Apply

You start by writing resources and their configuration in some `.tf`
file and run `terraform apply` to create them. Assuming you have the
credentials in place and enough permissions, Terraform will talk to the
AWS/GCP/Azure well know APIs and create things for you. When it's done,
it keeps all definitions of those resources in the state file.


## Plan

If you add new resources or change a field in a resource, you can
preview or diff the new configuration with `terrafom plan`. Terraform
at this point needs to reconcile 3 sources of information in order to
figure out what to do:

- the configuration in the TF files
- the last stored state
- the status of those resources according to the API

More on this reconcilation below.


## Init

Since Terraform is not for a single provider, it needs to know which
providers (and modules) it needs to install. It does this by parsing
your code to find which resources you are using and downloads the latest
providers (binary go-built files) into the `.terraform/` directory.

For example, unless told otherwise, terraform will use the
`hashicorp/aws` official provider for that resources that start with
the `aws_` prefix need. It will download and use the latest. If you need
a different version or want to pin it, one can say that explicitly in
the `terraform.required_providers` block.

To prepare these providers you need to run `terraform init`. This `init`
step also prepares the state where Terraform tracks resources.

To make things faster it creates a `.terraform.lock.hcl` lock file to
avoid scanning these files over and over again. So if you add resources
from a new provider, new modules or change the state file location you
need to re-run `terraform init`.


> **Note**
>
> If you are curious, Terraformm talks to its binary providers over
> [RPC](https://github.com/hashicorp/terraform-provider-aws/blob/main/main.go#L32-L36)
> and they expose an RPC server interface for it. Same for other
> plugins, like the remote backends.
