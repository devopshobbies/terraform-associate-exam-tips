<h3 style="color:yellowgreen">
- Usage: terraform console [options]
</h3>
This command provides an interactive command-line console for evaluating and experimenting with expressions. This is useful for testing interpolations before using them in configurations, and for interacting with any values currently saved in state.

<h3 style="color:yellowgreen">
provider syntax
</h3>
remember the syntax it's providers not provider

`terraform { required_providers {aws = ">=2.7.0"} }`

<h3 style="color:yellowgreen">where does terraform save the modules files</h3>

`.terraform/modules`

<h3 style='color:yellowgreen'>TF_LOG</h3>
TF_LOG levels are TRACE,DEBUG,INFO,WARN,ERROR,
TRACE is the most verboseand it is the default if TF_LOG is set to something other than a log level name.
to turning it of use TF_LOG=""

<h3 style='color:yellowgreen'>aws_eip</h3>
the aws_instance will be created first and then aws_eip will be created second due to the aws_eip's resource dependency of the aws_instance id

[https://learn.hashicorp.com/tutorials/terraform/dependencies](https://learn.hashicorp.com/tutorials/terraform/aws=dependency)

<h3 style='color:yellowgreen'>signs</h3>
when you are trying to apply something
the prefix +/- means that Terraform will destroy and recreate the resource rather than updating it in place. some attributes and resources can be updated i-place and are shown with the ~ prefix.

<h3 style='color:yellowgreen'>Terraform needs credentials</h3>
there are several different ways to configure credentials.

- integrated services, such as AWS IAM or Azure Managed Service Identity
- use environments variable
- directly in the provider block by hardcoding or using a variable

<h3 style='color:yellowgreen'>three provider in a question </h3>
look at the question below. and where will the resource be provisioned ?

<img src="img/terraform resource.png" width=600 heigh=600>

the resource above will be created in the default region of us-east-1. since the resource does signify and alternative rpovider configuration. if the resource needs to be created in one of the other declared regions. it hsould have looked like this, where aws signifies the provider name and "west" signifies the alias name as such

```
    resource "aws_instance" "vault"{
        provider = aws.west
        instance_type= "t3.micro"

        and so on !!
    }

```

<h3 style='color:yellowgreen'>Multiple Provider Configurations</h3>

You can optionally define multiple configurations for the `same provider` , and select which one to use on a per-resource or per-module basis. The primary reason for this is `to support multiple regions` for a cloud platform; other examples include targeting multiple Docker hosts, multiple Consul hosts, etc.

alias purpose in Terraform :
using the same provider with different configuration for different resources.

[https://www.terraform.io/language/providers/configuration](https://www.terraform.io/language/providers/configuration)

```
    # The default provider configuration; resources that begin with `aws_` will use
    # it as the default, and it can be referenced as `aws`.
    provider "aws" {
    region = "us-east-1"
    }

    # Additional provider configuration for west coast region; resources can
    # reference this as `aws.west`.
    provider "aws" {
    alias  = "west"
    region = "us-west-2"
    }

```

🌟 <h3 style='color:yellowgreen'>Provider dependencies</h3>

🌟 Provider dependencies are created in several different ways.

- explicit use of a provider block in configuration. optionally including a version constraint
- use of any resource belonging to particular provider in a resource or data block in the configuration
- existence of any resource instance belonging to particular provider in the current state

<h3 style='color:yellowgreen'>Hashicorp Style</h3>
 HashiCorp style conventions suggest you that align the equals sign for consecutive arguments for easing readability for configurations
 
 ami="abc123"
 andinstance_type ="t2.micro"
 [https://www.terraform.io/language/syntax/style](https://www.terraform.io/language/syntax/style)
