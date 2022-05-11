<h1 style='color:yellowgreen'>terraform validate  </h1>

Validation requires an initialized working directory with any referenced plugins and modules installed. If the directory is NOT initialized, it will result in an error.

terraform validate

Error: Could not load plugin
Plugin reinitialization required. Please run "terraform init".
Plugins are external binaries that Terraform uses to access and manipulate resources. The configuration provided requires plugins which can't be located,
don't satisfy the version constraints, or are otherwise incompatible.
Terraform automatically discovers provider requirements from your configuration, including providers used in child modules. To see the requirements and constraints, run "terraform providers".
Failed to instantiate provider "registry.terraform.io/hashicorp/aws" to obtain
schema: unknown provider "registry.terraform.io/hashicorp/aws"

<h1 style='color:yellowgreen'>terraform init -upgrade  </h1>

The -upgrade will `upgrade all previously-selected plugins to the newest version that complies with the configuration's version constraints`. This will cause Terraform to ignore any selections recorded in the dependency lock file, and to take the newest available version matching the configured version constraints.

<h1 style='color:yellowgreen'>save the terraform plan</h1>
The optional -out argument can be used to save the generated plan to a file for later execution with terraform apply, which can be useful when running Terraform in automation.

<h1 style='color:yellowgreen'>terraform parallelism</h1>

Terraform can limit the number of concurrent operations as Terraform walks the graph using the `-parallelism=n ` argument. The default value for this setting is` 10`. `This setting might be helpful if you're running into API rate limits.`


<h1 style='color:yellowgreen'>new provider</h1>
when ever you add a new provider you have to initialize terraform one more time to download the plugin.


<h1 style='color:yellowgreen'>what does tilda (~) mean in plan file</h1>
The prefix -/+ means that Terraform will destroy and recreate the resource, rather than updating it in-place. Some attributes and resources can be updated in-place and are shown with the ~ prefix.

<h1 style='color:yellowgreen'>what terraform init do</h1>
The terraform init command is used to initialize a working directory containing Terraform configuration files. This is the first command that should be run after writing a new Terraform configuration or cloning an existing one from version control. It is safe to run this command multiple times.  it will 
* initializes the backend configuration
* download the declared providers which are supported by hashicorp
* initializes downloaded and/or installed providers