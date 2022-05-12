<h1 style='color:yellowgreen'>Terraform refresh</h1>
Terraform apply, plan , destroy runs terraform refresh before starting
you can use -refresh=false flag for large infrastructures

<h1 style='color:yellowgreen'>Terraform plan -destroy </h1>
terraform plan -destroy
Plan command with -destroy option generates a plan to destroy all the known resources.
terraform destroy
This generates a plan to destroy all the known resources and ask for confirmation to go further to destroy resources.
terraform plan
Terraform plan is not applicable here. It is used to create an execution plan to determine what actions are necessary to achieve the desired state specified in the configuration files, which is not the case here. Here we want to destroy everything mentioned in the configuration files.
terraform destroy -auto-approve
Destroy command with  -auto-approve option does not generate a plan it directory destroys all resources given in the configuration files in non-intractive mode.
terraform validate
This command has no relation with resource destruction as it only validates the configuration files in a directory, referring only to the configuration and not accessing any remote services such as remote state, provider APIs, etc.

<h1 style='color:yellowgreen'>Terraform state backup  </h1>
 State Backups
 All terraform state subcommands that modify the state write backup files. The path of these backup file can be controlled with -backup.
 Subcommands that are read-only (such as list) do not write any backup files since they aren't modifying the state.
 Note that backups for state modification can not be disabled. Due to the sensitivity of the state file, Terraform forces every state modification command to write a backup file. You'll have to remove these files manually if you don't want to keep them around.

<h1 style='color:yellowgreen'>Terraform taint  </h1>
 The terraform taint command manually marks a Terraform-managed resource as tainted, forcing it to be destroyed and recreated on the next apply.
 This command will not modify infrastructure, but does modify the state file in order to mark a resource as tainted. Once a resource is marked as tainted, the next plan will show that the resource will be destroyed and recreated and the next apply will implement this change.

<h1 style='color:yellowgreen'>Terraform Show  </h1>

 The terraform show command is used to provide human-readable output from a `state` or `plan` file. This can be used to inspect a plan to ensure that the planned operations are expected, or to inspect the current state as Terraform sees it.



<h1 style='color:yellowgreen'>Terraform Providers  </h1>
 The terraform providers command prints information about the providers used in the current configuration.
 This command gives an overview of all of the current dependencies, as an aid to understanding why a particular provider is needed.
 terraform providers
 .
 ├── provider.aws
 ├── provider.azurerm
 ├── provider.google
 └── provider.vmc
 
   <h1 style='color:yellowgreen'>Removing a resource form the state   </h1>
 Removing a Resource from State
 Items removed from the Terraform state are not physically destroyed. Items removed from the Terraform state are only no longer managed by Terraform. For example, if you remove an AWS instance from the state, the AWS instance will continue running, but terraform plan will no longer see that instance.
 There are various use cases for removing items from a Terraform state file. The most common is refactoring a configuration to no longer manage that resource (perhaps moving it to another Terraform configuration/state).
 The state will only be saved on successful removal of all addresses. If any specific address errors for any reason (such as a syntax error), the state will not be modified at all.
 This command will output a backup copy of the state prior to saving any changes. The backup cannot be disabled. Due to the destructive nature of this command, backups are required.

 Command:  `state rm`
 The terraform state rm command is used to remove items from the Terraform state. This command can remove single resources, single instances of a resource, entire modules, and more.
 Example: Remove a Resource
 The example below removes the aws_instance resource named vm:

`terraform state rm 'aws_instance.vm'`


   <h1 style='color:yellowgreen'>Terraform state mv </h1>

`terraform state mv [options] SOURCE DESTINATION`
This command will move an item matched by the address given to the destination address. This command can also move to a destination address in a completely different state file.
This can be used for simple resource renaming, moving items to and from a module, moving entire modules, and more. And because this command can also move data to a completely new state, it can also be used for refactoring one configuration into multiple separately managed Terraform configurations.
This command will output a backup copy of the state prior to saving any changes. The backup cannot be disabled. Due to the destructive nature of this command, backups are required.
If you're moving an item to a different state file, a backup will be created for each state file.

        
   <h1 style='color:yellowgreen'>How rename a resource on Terraform </h1>
 Rename a Resource
 Terraform tracks resources by their name. If you change the name, you have created a new resource and deleted the old resource. There is no way for Terraform to tell you changed the name of an existing resource since its all just text. Hence even if the user has just changed the resource name, Terraform will delete the existing VM and create a new VM with a new name.
 How to rename a resource instead of deleting it?
 Renaming a resource in Terraform without deleting and creating it is a 2 step process.
 For example, to rename the aws_instance resource named test to tool_instance:
 1)  Define your resource block with a new name.

 # Something
 ```
 resource "aws_instance" "tool_instance" {
       ami           = "ami-05794d4ef61b053f2"
   instance_type = "t2.micro"
 }
 ```

 2)  Use the Terraform move command.
 # Execute the Terraform move command 
 # by passing your old resource name first, followed by your new resource name.
 $ `terraform state mv 'aws_instance.test' 'aws_instance.tool_instance'`
 To verify it, run, Terraform plan command after above given two steps, and make sure terraform plan says - "No changes. Infrastructure is up-to-date." same as example given below -
 $ `terraform plan`
 Acquiring state lock. This may take a few moments...
 Refreshing Terraform state in-memory prior to plan...
 The refreshed state will be used to calculate this plan, but will not be
` persisted to local or remote state storage.
 aws_instance.tool_instance: Refreshing state... [id=i-018d635d0ab1a2f4b]`
 ------------------------------------------------------------------------
` No changes. Infrastructure is up-to-date.`
 This means that Terraform did not detect any differences between your
 configuration and real physical resources that exist. As a result, no
 actions need to be performed.
 
     
<h1 style='color:yellowgreen'>Terraform 0.12 and init </h1> 
 Plugin Installation
 During init, Terraform searches the configuration for both direct and indirect references to providers and attempts to load the required plugins.
 Backend Initialization
 During init, the root configuration directory is consulted for backend configuration and the chosen backend is initialized using the given configuration settings.
 Child Module Installation
 During init, the configuration is searched for module blocks, and the source code for referenced modules is retrieved from the locations given in their source arguments.
 Community/Third-party Plugins (Terraform 0.12)
 Anyone can develop and distribute their own Terraform providers. These third-party providers must be manually installed, since terraform init cannot automatically download them.
 Install third-party providers by placing their plugin executables in the user plugins directory. The user plugins directory is in one of the following locations, depending on the host operating system:
 Once a plugin is installed, terraform init can initialize it normally. You must run this command from the directory where the configuration files are located.
 Providers distributed by HashiCorp can also go in the user plugins directory. If a manually installed version meets the configuration's version constraints, Terraform will use it instead of downloading that provider. This is useful in airgapped environments and when testing pre-release provider builds.
 Community/Third-party Plugins (Terraform 0.13)     &lt;- New
 With Terraform 0.13, terraform init will automatically download and install partner and community providers in the HashiCorp Terraform Registry, following the same clear workflow as HashiCorp-supported official providers. These improvements to the ecosystem will benefit Terraform users and provider developers alike.
 NOTE:  Terraform recently announced Terraform 0.13, it can do the automatic installation of third-party (community) providers.
 Hence, in the exam, you can be asked about terraform init command behavior in Terraform 0.13, then you should also mark -&gt; Downloads and installs community as well as HashiCorp distributed provider's plugin so that it can later be executed during terraform plan and apply.
 New -&gt; Click here to know Automatic Installation of Third-Party Providers with Terraform 0.13 
 


<h3 style="color:yellowgreen">
- Usage: terraform console [options]
</h3>
This command provides an interactive command-line console for evaluating and experimenting with expressions. This is useful for testing interpolations before using them in configurations, and for interacting with any values currently saved in state.



<h3 style='color:yellowgreen'>Terraform commands</h3>

`Terraform taint `command manually marks a Terraform-managed resource as tainted, forcing it to be destroyed and recreated on the next apply.
`Terraform workspace` select command is used to choose a different workspace to use further operations. 
`Terraform login` command can be used to automatically obtain and save an api token for Terraform Cloud, Terraform Enterprise , or any other host that offers Terraform services.

<h3 style='color:yellowgreen'>Terraform replace</h3>

The` terraform apply -replace `command manually marks a Terraform-managed resource for replacement, forcing it to be destroyed and recreated on the apply execution.

You could also use terraform destroy -target <virtual machine> and destroy only the virtual machine and then run a terraform apply again.

IMPORTANT - PLEASE READ

This command replaces terraform taint, which was the command that would be used up until 0.15.x. You may still see terraform taint on the actual exam until it is updated.

🌟🌟🌟 <h1 style='color:yellowgreen'>Terraform state backup  </h1>

By default, terraform init downloads plugins into a subdirectory of the working directory, `.terraform/providers `so that each working directory is self-contained.
🌟🌟🌟 <h1 style='color:yellowgreen'>to delete all resources</h1> 
‍
- terraform destroy 
- terraform apply -destroy

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform fmt -recursive</h1> 
By default, fmt scans the current directory for configuration files and formats them according to the HCP canonical style and format. However, if you need it to also scan and format files in sub-directories, you can use the -recursive flag to instruct terraform fmt to also process files in subdirectories.