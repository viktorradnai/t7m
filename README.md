# TFMR - A Terraform Helper

TFMR is a simple wrapper around Terraform that helps manage multiple instances of
the same terraform stack using variable files and uniquely prefixed remote states.
It exists to reduce the need to type long command line arguments.

This script is designed to make it easier to spin up multiple copies of the same
Terraform stack, such as dev / staging / prod, or two replicas in different regions.
Using modules from within a whole-environment stack for this purpose would make it
harder to delegate the management of a stack to the developers who look after the code.

The script exists because remote state configuration cannot be configured using
ordinary terraform variables, and GCS bucket names are globally unique, and changing
the remote state config or the provider project requires a terraform init.

Workspaces would accomplish most of what this script does, but they have some limitations.
All the state files are stored together in the same location, therefore an user who can
apply the dev stack will also need write access to the prod remote state file.
Users of workspaces must also take care to select the correct one before running plan/apply.

## Examples:

```
    tfmr foo/bar plan
    tfmr foo/bar apply -auto-approve
```

In the above examples, the vars file `foo/bar.tfvars` will be used and must exist.
The remote state prefix will be `foo/bar` and the state will end up in the bucket set in the
provider block, with a path of `/foo/bar/terraform.tfstate`.

TFMR has only been tested on Google Cloud so far.
