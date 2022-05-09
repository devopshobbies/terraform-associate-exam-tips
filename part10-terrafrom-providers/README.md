<h1 style='color:yellowgreen'>terraform upgrade provider </h1>

To upgrade to the latest acceptable version of each provider, run `terraform init -upgrade`. This command also upgrades to the latest versions of all Terraform modules.

<h1 style='color:yellowgreen'>Terraform provider dependencies</h1>
 Provider dependencies are created in several different ways:
       * Explicit use of a provider block in configuration, optionally including a version constraint.
       * Use of any resource belonging to a particular provider in a resource or data block in configuration.
       * Existence of any resource instance belonging to a particular provider in the current state. For example, if a particular resource is removed from the configuration, it continues to create a dependency on its provider until its instances have been destroyed.
 terraform providers  command gives an overview of all of the current dependencies, as an aid to understanding why a particular provider is needed.
 
<h1 style='color:yellowgreen'>Terraform provider 0.12</h1>
 Provider version constraints can also be specified using a version argument within a provider block, but that simultaneously declares a new provider configuration that may cause problems particularly when writing shared modules. For that reason, we recommend using the required_providers block as described above, and not using the version argument within provider blocks. version is still supported for compatibility with older Terraform versions.
 # Recommend

``` 
terraform {
       required_providers {
         aws = "~&gt; 3.0"
   }
 }
 # Only supported for compatibility with older Terraform versions
 provider "aws" {
       version = "~&gt; 3.0"
 }
 ```
 The version meta-argument specifies a version constraint for a provider, and works the same way as the version argument in a required_providers block. The version constraint in a provider configuration is only used if required_providers does not include one for that provider.
 Terraform does not recommend using the version argument in provider configurations. In Terraform 0.13 and later, version constraints should always be declared in the required_providers block.
 Note: The version meta-argument made sense before Terraform 0.13, since Terraform could only install providers that were distributed by HashiCorp. Now that Terraform can install providers from multiple sources, it makes more sense to keep version constraints and provider source addresses together.
 Update: New in Terraform 0.13
 The name = { source, version } syntax for required_providers was added in Terraform 0.13. Previous versions of Terraform used a version constraint string instead of an object (like mycloud = "~&gt; 1.0"), and had no way to specify provider source addresses. If you want to write a module that works with both Terraform v0.12 and v0.13, see v0.12-Compatible Provider Requirements below.
 Now, a provider requirement consists of a local name, a source location, and a version constraint:
```
 terraform {
       required_providers {
         mycloud = {
           source  = "mycorp/mycloud"
       version = "~&gt; 1.0"
     }
   }
 }
 ```
 Each argument in the required_providers block enables one provider. The key determines the provider's local name (its unique identifier within this module), and the value is an object with the following elements:
 * source - the global source address for the provider you intend to use, such as hashicorp/aws.
 * version - a version constraint specifying which subset of available provider versions the module is compatible with.
