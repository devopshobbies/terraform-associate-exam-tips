<h1 style='color:yellowgreen'>what if you remove the version from the module which you used before and run terraform init once again </h1>
 When using modules installed from a module registry, HashiCorp recommends explicitly constraining the acceptable version numbers to avoid unexpected or unwanted changes. The version argument accepts a version constraint string. Terraform will use the newest installed version of the module that meets the constraint; if no acceptable versions are installed, it will download the newest version that meets the constraint.
 
A version number that meets every applicable constraint is considered acceptable.

Terraform consults version constraints to determine whether it has acceptable versions of itself, any required provider plugins, and any required modules. For plugins and modules, it will use the newest installed version that meets the applicable constraints.

To test this, I ran a terraform init with the code as shown in the file:
```
$ terraform init

Initializing modules...

Downloading hashicorp/consul/aws 0.0.5 for consul...

- consul in .terraform\modules\consul

- consul.consul_clients in .terraform\modules\consul\modules\consul-cluster

- consul.consul_clients.iam_policies in .terraform\modules\consul\modules\consul-iam-policies

- consul.consul_clients.security_group_rules in .terraform\modules\consul\modules\consul-security-group-rules

- consul.consul_servers in .terraform\modules\consul\modules\consul-cluster

- consul.consul_servers.iam_policies in .terraform\modules\consul\modules\consul-iam-policies

- consul.consul_servers.security_group_rules in .terraform\modules\consul\modules\consul-security-group-rules

Then I removed the constraint from the configuration file and ran a terraform init again:

$ terraform init

Initializing modules...

Initializing the backend...

Initializing provider plugins...

- Reusing previous version of hashicorp/aws from the dependency lock file

- Reusing previous version of hashicorp/template from the dependency lock file
```

`Terraform did not download a newer version of the module. It reused the existing one.`

<a href="https://www.terraform.io/docs/configuration/blocks/modules/syntax.html#version" target="_blank">https://www.terraform.io/docs/configuration/blocks/modules/syntax.html#version</a>

<a href="https://www.terraform.io/docs/language/expressions/version-constraints.html" target="_blank">https://www.terraform.io/docs/language/expressions/version-constraints.html</a>

<h1 style='color:yellowgreen'>how use terraform modules  </h1>

if the modules is on the public registry
```
module "consul" {
  source = "hashicorp/consul/aws"
  version = "0.1.0"
}

```
For modules hosted in other registries, prefix the source address with an additional <HOSTNAME>/ portion, giving the hostname of the private registry:
```
module "consul" {
  source = "app.terraform.io/example-corp/k8s-cluster/azurerm"
  version = "1.1.0"
}
```

Terraform will recognize unprefixed github.com URLs and interpret them automatically as Git repository sources.
```
module "consul" {
  source = "github.com/hashicorp/example"
}
```
The above address scheme will clone over HTTPS. To clone over SSH, use the following form:
```
module "consul" {
  source = "git@github.com:hashicorp/example.git"
}
``` 
Terraform will recognize unprefixed bitbucket.org URLs and interpret them automatically as BitBucket repositories:
```
module "consul" {
  source = "bitbucket.org/hashicorp/terraform-consul-aws"
}
```

Arbitrary Git repositories can be used by prefixing the address with the special git:: prefix. After this prefix, any valid Git URL can be specified to select one of the protocols supported by Git.

For example, to use HTTPS or SSH:
```
module "vpc" {
  source = "git::https://example.com/vpc.git"
}

module "storage" {
  source = "git::ssh://username@example.com/storage.git"
}
```

<h3>Selecting a Revision</h3>
By default, Terraform will clone and use the default branch (referenced by HEAD) in the selected repository. You can override this using the ref argument. The value of the ref argument can be any reference that would be accepted by the git checkout command, such as branch, SHA-1 hash (short or full), or tag names. The Git documentation contains a complete list.

```
# referencing a specific release
module "vpc" {
  source = "git::https://example.com/vpc.git?ref=v1.2.0"
}

# referencing a specific commit SHA-1 hash
module "storage" {
  source = "git::https://example.com/storage.git?ref=51d462976d84fdea54b47d80dcabbf680badcdb8"
}
```
<h3>Fetching archives over HTTP</h3>

```
module "vpc" {
  source = "https://example.com/vpc-module.zip"
}
```
<h3>S3 bucket</h3>

```
module "consul" {
  source = "s3::https://s3-eu-west-1.amazonaws.com/examplecorp-terraform-modules/vpc.zip"
}

```

<h3>GCS bucket</h3>

```
module "consul" {
  source = "gcs::https://www.googleapis.com/storage/v1/modules/foomodule.zip"
}

```
<h1 style='color:yellowgreen'>how create public modules  </h1>
The list below contains all the requirements for publishing a module. Meeting the requirements for publishing a module is extremely easy. The list may appear long only to ensure we're detailed, but adhering to the requirements should happen naturally.

GitHub. The module must be on GitHub and must be a public repo. This is only a requirement for the public registry. If you're using a private registry, you may ignore this requirement.

Named `terraform-<provider>-<name>`. Module repositories must use this three-part name format, where` <name>` reflects the type of infrastructure the module manages and `<provider>` is the main provider where it creates that infrastructure. The `<name> `segment can contain additional hyphens. Examples: terraform-google-vault or terraform-aws-ec2-instance.

Repository description. The GitHub repository description is used to populate the short description of the module. This should be a simple one-sentence description of the module.

Standard module structure. The module must adhere to the standard module structure. This allows the registry to inspect your module and generate documentation, track resource usage, parse submodules and examples, and more.

x.y.z tags for releases. The registry uses tags to identify module versions. Release tag names must be a semantic version, which can optionally be prefixed with a v. For example, v1.0.4 and 0.9.2. To publish a module initially, at least one release tag must be present. Tags that don't look like version numbers are ignored.

<h1 style='color:yellowgreen'>which commands install and update the modules </h1>
 Both the terraform get and terraform init commands will install and update modules. The terraform init command will also initialize backends and install plugins.

<h1 style='color:yellowgreen'>module versioning </h1>

 Version constraints are supported only for modules installed from a `module registry`, such as the `public Terraform Registry` or `Terraform Cloud's private module registry`. Other module sources can provide their own versioning mechanisms within the source string itself, or might not support versions at all. In particular, modules sourced from local file paths do not support version; since they're loaded from the same source repository, they always share the same version as their caller.

<h1 style='color:yellowgreen'>who is the calling module </h1>

file main.tf is:
```
module "server" {
    source = "./app-cluster"
    servers= 5
}
```
the main.tf file is the calling module and the app-cluster is a child module.

 To call a module means to include the contents of that module into the configuration with specific values for its input variables. Modules are called from within other modules using module blocks. A module that includes a module block like this is the calling module of the child module.

The label immediately after the module keyword is a local name, which the calling module can use to refer to this instance of the module.
