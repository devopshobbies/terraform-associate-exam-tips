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
