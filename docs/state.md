# State

The all important state file `terraform.tfstate` is the "database" Terraform uses to track which
resources were created with it.


## Church and state

The golden rule is:

![Do not the cat](https://raw.githubusercontent.com/jpedro/levelup-terraform/master/.github/assets/images/donot.jpg)

**DO NOT EDIT THE STATE MANUALLY.**

Even Terraform CLI commands that manipulate the state
should be used with great caution only. If you need to test things out,
back up the state file first. Only when you are certain that your change works
do it on the original file.

> **Note**
>
> For real applications, make sure the state file is stored with a remote backend
> in versioned blob storage (e.g. an S3 bucket, or Terraform Cloud).


## What

Tools like CloudFormation and Azure Resource Manager keep the
state internal, and it does not survive after a run of the tool.
With Terraform, the state lives in a file, by default a file in
your local file system.

Unless you are testing and playing with Terraform, this state file
should live in versioned remote storage. Typical locations for it are
a file object inside S3/GCP bucket or inside an Azure container blob.

This state file tracks the latest state for each resource that was successfully
changed (applied). If resource A was changed, but resource B failed,
next time you try to `apply` only resource B will be changed.


## But how does it look

Here's an example with a single resource of type `random_pet`:
```json
{
  "version": 4,
  "terraform_version": "1.4.6",
  "serial": 4,
  "lineage": "51ef1aea-c982-c64b-6999-7fafc14b505a",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "random_pet",
      "name": "name",
      "provider": "provider[\"registry.terraform.io/hashicorp/random\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "id": "super-platypus",
            "keepers": null,
            "length": 2,
            "prefix": null,
            "separator": "-"
          },
          "sensitive_attributes": []
        }
      ]
    }
  ],
  "check_results": null
}
```

## Reconciliation


## Manipulation

Terraform's `state` command supports the following subcommands:

```
list                List resources in the state
mv                  Move an item in the state
pull                Pull current state and output to stdout
push                Update remote state from a local state file
replace-provider    Replace provider in the state
rm                  Remove instances from the state
show                Show a resource in the state
```

The `list` and `show` subcommands are mere read operations to inspect the state,
all the other commands should be used with great caution!
