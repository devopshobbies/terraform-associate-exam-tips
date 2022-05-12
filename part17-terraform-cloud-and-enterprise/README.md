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
🌟🌟🌟 <h1 style='color:yellowgreen'>terraform enterprise vsc mapping</h1>
In Terraform Enterprise, a workspace can be mapped to how many VCS repos?

A workspace can only be configured to` a single VCS repo`, however, multiple workspaces can use the same repo, if needed. A good explanation of how to configure your code repositories can be found here.

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform workspace in OSS, Cloud, Enterprise versions</h1>

Workspaces, managed with the terraform workspace command, `isn't the same thing as Terraform Cloud's workspaces`. Terraform Cloud workspaces act more like completely separate working directories.

CLI workspaces (OSS) are just alternate state files.

🌟🌟🌟 <h1 style='color:yellowgreen'>only in the Enterprise editions</h1>

While there are a ton of features that are available to open source and Cloud users, there are still a few features that are part of the Enterprise offering which is geared towards enterprise requirements. With the introduction of Terraform Cloud for Business, almost all features are now available for a hosted Terraform deployment. To see what specific features are part of Terraform Cloud and Terraform Enterprise, check out this link.

Information about Answers:

  - Private Network Connectivity is available for Terraform Enterprise, since it is installed locally in your data center or cloud environment. However, it is also available in Terraform Cloud for Business since you can use a self-hosted agent to communicate with local/private hosts in your environment.

  - SAML/SSO is available in BOTH Terraform Enterprise and Terraform Cloud for Business, therefore it is NOT exclusive to just Terraform Enterprise.

  - Private Module Registry is available in every version of Terraform except for Open-Source. Therefore it is NOT exclusive to Terraform Enterprise

  - Locally hosted installation is available for Terraform Enterprise, but technically you install Terraform open-source on your local machine or server, therefore it is NOT exclusive to Terraform Enterprise.

  - `Clustering is the ONLY answer that is available ONLY in Terraform Enterprise`. You can't cluster Terraform open-source, and the other options are hosted solutions. This makes clustering the only correct answer. Note that Clustering was available for Enterprise for a while, then HashiCorp removed it. As of January 15, 2021, it's back and you can read more about it at this link.

🌟🌟🌟 <h1 style='color:yellowgreen'>what feature of terraform cloud is <b>NOT</b> free to customers</h1>
team management 
sentinel policy as code 
run tasks 
policy enforcement
bronze support 
cost estimation

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform cloud features</h1>
remote runs
private module registry 
workspace management
vcs connection
remote state

🌟🌟🌟 <h1 style='color:yellowgreen'>Terraform cli and cloud workspaces</h1>

correct statements regarding workspaces

- Terraform cloud maintains the state version and run history for each workspace
- Terraform Cloud manages infrastructure collections with a workspace whereas cli manages collections of infrastructure resources with a persistent working directory 
- cli workspaces are alternative state files in the same working directory

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform apply -refresh-only</h1>

The` terraform apply -refresh-only` command is used to `reconcile the state` Terraform knows about (via its state file) with the real-world infrastructure. This can be used `to detect any drift from the last-known state`, `and to update the state file.`