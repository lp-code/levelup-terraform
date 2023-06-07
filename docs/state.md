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


## Advanced state manipulation 

### Import existing resources

Since not every deployment is going to be a greenfield deployment and there is a big chance that projects might have existing resources Terraform offers the `import` command to bring those resources into the state file. 
Note that using this command will only update the state and will not generate any configurations for the existing resources. This means that you have to write configurations that matches the resources you are going to import. 

```
terraform import [options] ADDRESS ID
```
The `Address` must be Terraform resource address like `aws_instance.foo` or `module.foo.aws_instance.bar` while the `ID` represents the unique resource ID which dependent on the resource type.

For instance if you have an existing EC2 instance with the name `web_server` you need to write configurations to match the instance and all its settings. 

```
resource "aws_instance" "web_server" {
  ami           = "ami-09fd16644beea3565"
  instance_type = "t2.micro"

  tags = {
    Name = "web_server"
  }
}
```

Running `terraform plan` now will show that Terraform will try to create the new EC2 instance but we can tell Terraform to not to do so by importing the resource like this:

```
terraform import 'aws_instance.web_server' i-abcd1234
```


### Renaming existing resources

Sometimes you might need to change resource name in the Terraform configurations and by simply doing so Terraform will plan to destroy and recreate the resource in order to update the state file. You can update the name in the configuration and use the `state mv` command to update the state file and avoid the recreation process. 

```
terraform state mv [options] SOURCE DESTINATION
terraform state mv 'aws_instance.web_server' 'aws_instance.db'
```

### Remove resource management from Terraform    
Let's assume that you have a database instance provisioned by Terraform and you want to tell Terraform to forget about it without destroying it, then you can use the `state rm command` to achieve that

```
terraform state rm [options] ADDRESS
terraform state rm aws_db_instance.foo
```

### Refresh the state

It is best practice to update resources only through configurations and not applying any changes manually. However, if you changed the name of an instance from the AWS management console Terraform will notice the resource name has been changed and it will plan to revert the change to match the configuration. If you need to keep the new name you only need to update the configuration to match the new name and run ` apply -refresh-only`. Using `plan,apply` command in the normal mode refresh the state by default so this can also be used to achieve the same result. 
NB: Even though the `plan` command refresh the state the state file is updated only by running once you apply the changes