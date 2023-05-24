# State

The all important state is the "database" Terraform uses to track which
resources were created with it.


## Church and state

The golden rule is:

![Do not the cat](https://raw.githubusercontent.com/jpedro/levelup-terraform/master/.github/assets/images/donot.jpg)

**DO NOT EDIT THE STATE MANUALLY.**

Like a beautiful person, you can look but you cannot touch.

Even consentful cli commands that manipulate the state outside writing
TF files should be handled carefully. If you need to test things out,
copy the state file to another file, use that as storage and test
things in there. When you are sure do it on the original file.

> **Note**
>
> Make sure the state files are versioned so you can detraumatised them.


## What

Tools like CloudFormation and Azure Resource Manager keep that database
internal. With terraform it lives in a file. By default is a file in
your local file system.

Unless you are testing and playing with Terraform, this state file
should live in versioned remote storage. Typical locations for it are
a file object inside S3/GCP bucket or inside an Azure container blob.

This state file tracks the latest state for each resource successful
changed (applied). If resource A was changed, but resource B failed,
next time you try to `apply` onlyresource B will be changed.


## But how does it look


## Reconciliation


## Manipulation
