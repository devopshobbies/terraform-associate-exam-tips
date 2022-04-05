<h1 style='color:yellowgreen'>exam Tips for Terraform state</h1>

<h3 style='color:yellowgreen'>Terraform store</h3>
by default changes are stored in the local file named terraform.tfstate under the current working directory

<h3 style='color:yellowgreen'>workspace</h3>

workspaces are technically equivalent to renaming your state file.they are not anymore complex than than. Terraform wraps this simple notion with a set of protections and support for remote state . `For local state`, Terraform stores the `workspace state` in a direcotry called` terraform.tfstate.d`. This directory should be treated similarly to local-only terraform.tfstate


Named workspaces allow conveniently switching between multiple instances of a single configuration within its single backend . they are conveninet in a number of situations. but cannot sole all problems. a common use for multiple workspaces is to create a parallel, distinct copy of a set of infrastructure in order to test a set of changes before modifying the main production infrastructure. 

<h3 style='color:yellowgreen'>Terraform state purpose</h3>

terraform uses the state to track metadata , such as resource dependencies.
terraform stores a cache of the attribute values for all resources in the state for performance improvement
terraform uses the state to map configuration to resource in the real world 

<h3 style='color:yellowgreen'>sensitive true</h3>

output can be marked as containing sensitive material using the option sensitive argument . setting an output 
value in the root module as sensitive prevents Terraform frm showing its value in the list of outputs at the end of terraform apply. it might still be shown in the cli output for other reasons. like if the value is referenced in an expression for a resource argument. sensitive output values are still recorded in the state, and so will be visible to anyone who is able to access the state data

<h3 style='color:yellowgreen'>remote state</h3>
terraform state can contain sensitive data .hence while using remote state Terraform does not persist the state to the local disk ; instead the state is only ever held in memory when used by Terraform

<h3 style='color:yellowgreen'>Terraform backend </h3>
each  Terraform configuration has an associated backend that defines how operations are executed and where persistent data such as the Terraform state are stored. to ensure that terraform workspace names are stored correctly and safely in all backends , the workspace name must be valid to use in a URL path segment without escaping.

<h3 style='color:yellowgreen'>Terraform state locking</h3>
if backend supports state locking , Terraform will lock your state for all operations that could write state.this prevents others from acquiring the lock and potentially corrupting your state.

<h3 style='color:yellowgreen'>Terraform performance</h3>

in additional to basic resource mapping, Terraform stores a cache of the attribute values for all resources in the state. this is the most optional feature of Terraform state and is done only as performance improvement.
when running a terraform plan, Terraform must know the current state of resources in order to effectively determine changes that it needs to make to reach your desired configuration. for small infrastructures, Terraform can query your providers and sync the lates attributes from all your resources. this is the default behavior of Terraform: for every plan and apply , Terraform will sync all resources in your state for larger infrastructures, querying every resource is too slow. Many cloud providers do not provide APIs to query multiple resources at once and the round trip time for each resource in hundreds of milliseconds . on top of this , cloud providers almost always have API rate limiting so Terraform can only request a certain number of resources in a period of time . larger users of terraform make heavy use of the `-refresh=false `flag as well as the `-target` flag in orrder to work around this. in these scenarios the cached state is treated as the record of truth. However

