<h1 style='color:yellowgreen'>provider dependencies </h1>
8 existence of any resource instance belonging to particular provider in the current state 
Use of any resource belonging to a particular provider in a resource or data block in the configuration. 
Explicit use of a provider block in configuration, optionally including a version constraint. 
 The existence of a provider plugin found locally in the working directory does not itself create a provider dependency. The plugin can exist without any reference to it in the terraform configuration.
 <h1 style='color:yellowgreen'>terraform plugins</h1>
By default, terraform init downloads plugins into a subdirectory of the working directory, .terraform/plugins,  so that each working directory is self-contained

 <h1 style='color:yellowgreen'>terraform environment variables</h1>
  Environment variables can be used to set variables. The environment variables must be in the format TF_VAR_name and this will be checked last for a value. For example:

```  
export TF_VAR_region=us-west-1

export TF_VAR_ami=ami-049d8641

export TF_VAR_alist='[1,2,3]'

export TF_VAR_amap='{ foo = "bar", baz = "qux" }'

```

<h1 style='color:yellowgreen'>terraform custom provider</h1>
Each time a new provider is added to configuration -- either explicitly via a provider block or by adding a resource from that provider -- Terraform must initialize the provider before it can be used. Initialization downloads and installs the provider's plugin so that it can later be executed

<h1 style='color:yellowgreen'>why is it a good idea to declare the required version of a provider in a terraform configuration file </h1>
 Providers are plugins released on a separate rhythm from Terraform itself, and so they have their own version numbers. For production use, you should constrain the acceptable provider version via configuration. This helps to ensure that new versions with potentially breaking changes will not be automatically installed by terraform init in the future.

<h1 style='color:yellowgreen'>terraform specific settings and behaviors</h1>
 The special terraform configuration block type is used to configure some behaviors of Terraform itself, such as requiring a minimum Terraform version to apply your configuration.

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform enterprise installation without internet</h1>

A Terraform Enterprise install that is provisioned on a network that does not have Internet access is generally known as an `air-gapped` install. These types of installs require you to pull updates, providers, etc. from external sources vs. being able to download them directly.

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform cloud supported VCS provider</h1>

Terraform Cloud supports the following VCS providers as of January 2022:

  - GitHub

  - GitHub.com (OAuth)

  - GitHub Enterprise

  - GitLab.com

  - GitLab EE and CE

  - Bitbucket Cloud

  - Bitbucket Server

  - Azure DevOps Server

  - Azure DevOps Services
  

 <h1 style='color:yellowgreen'>terraform configuration block and specific version</h1>
 For production use, you should constrain the acceptable provider versions via configuration file to ensure that new versions with breaking changes will not be automatically installed by terraform init in the future. When terraform init is run without provider version constraints, it prints a suggested version constraint string for each provider
 
For example:
```
 terraform {
     
  required_providers {
      
    aws = "&gt;= 3.1.0"
    
  }
  
}
```
