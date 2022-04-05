<h3 style="color:yellowgreen">
provider syntax
</h3>
remember the syntax it's providers not provider

`terraform { required_providers {aws = ">=2.7.0"} }`

<h3 style="color:yellowgreen">where does terraform save the modules files</h3>

`.terraform/modules`

<h3 style='color:yellowgreen'>signs</h3>
when you are trying to apply something
the prefix +/- means that Terraform will destroy and recreate the resource rather than updating it in place. some attributes and resources can be updated i-place and are shown with the ~ prefix.

<h3 style='color:yellowgreen'>why this option?</h3>
terraform {
    required_version = ">=0.12"
    }

You can use required_version to ensure that a user deploying infrastructure is using Terraform 0.12 or greater , due to the vast number of changes that were introduced. as a result, many previously written configurations had to be converted or rewritten.
