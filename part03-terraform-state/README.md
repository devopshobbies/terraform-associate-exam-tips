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

<h3 style='color:yellowgreen'>locked file :(</h3>
Due to some problem , you end up locking the Terraform state . now your team members are not able to run terraform apply command to make any infrastructure changes. how to fix it ?

Terraform has a force-unlock command to manually unlock the state if unlocking failed.
be very careful with this command . if you unlock the sate when someone else is holding the lock it could cause mulitple writers. Force unlock should only be used to unlock your own lock in the situation where automatic unlocking failed.to protect you , the force-unlock command requires a unique lock ID. Terraform will output this lock ID if unlocking fails. this lock ID acts as a nonce , ensuring the locks and unlocks target correct lock.

remember`-lock`flag disable state locking for most commands. and it is not recommended.

<h3 style='color:yellowgreen'>secure Terraform state</h3>
Storing state remotely can provide better security . as the Terraform 0.9, Terraform does not persis state to the local disk when remote state is in use, and some backends can be configured to encrypt the state data at rest.
you can use TLS in transit to remote state.

<h3 style='color:yellowgreen'>workspace directory </h3>
you have created a new workspace called DEV under directory /home/quiz_expert/iac/ and initialized terraform by running terraform init command with local backend.

if you run terraform apply , where does Terraform will write its sate data ?

`/home/quiz_expert/iac/terraform.tfstate.d/DEV/terraform.tfstate`

For local state , Terraform stores the workspace states in a directory called terraform.tfsate.d this direcotry should be treated similarly to local-only terraform.tfsate
so it will be like `terraform.tfstate.d/{your workspace name}/ terraform.tfstate`


<h3 style='color:yellowgreen'>Reconciling real-world drifts</h3>

Prior to a plan or apply operation, Terraform does a refresh to update the state file with real-world status. You can also do a refresh any time with` terraform refresh`:
what Terraform is doing here is reconciling the resources tracked by the state file with the real world. it does this by querying your infrastructure providers to find out what's actually running and the current configuration. and updating the state file with this new information.Terraform is designed to co-exist with other tools as well as manually provisioned resourcs and so it only refreshes resources under its management