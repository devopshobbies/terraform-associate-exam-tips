<h1 style='color:yellowgreen'>exam Tips for Terraform state</h1>

<h3 style='color:yellowgreen'>Terraform store</h3>
by default changes are stored in the local file named terraform.tfstate under the current working directory

<h3 style='color:yellowgreen'>workspace</h3>

workspaces are technically equivalent to renaming your state file.they are not anymore complex than than. Terraform wraps this simple notion with a set of protections and support for remote state . `For local state`, Terraform stores the `workspace state` in a direcotry called` terraform.tfstate.d`. This directory should be treated similarly to local-only terraform.tfstate



