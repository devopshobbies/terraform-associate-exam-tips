<h3 style="color:yellowgreen">
provider syntax
</h3>
remember the syntax it's providers not provider

`terraform { required_providers {aws = ">=2.7.0"} }`

<h3 style="color:yellowgreen">where does terraform save the modules files</h3>

`.terraform/modules`

<h3 style='color:yellowgreen'>signs</h3>
when you are trying to apply something
the prefix +/- means that Terraform will destroy and recreate the resource rather than updating it in place. some attributes and resources can be updated i-place and are shown with the ~ prefix.

<h3 style='color:yellowgreen'>why this option?</h3>
terraform {
    required_version = ">=0.12"
    }

You can use required_version to ensure that a user deploying infrastructure is using Terraform 0.12 or greater , due to the vast number of changes that were introduced. as a result, many previously written configurations had to be converted or rewritten.


<h3 style='color:yellowgreen'>aws_eip</h3>
the aws_instance will be created first and then aws_eip will be created second due to the aws_eip's resource dependency of the aws_instance id

[https://learn.hashicorp.com/tutorials/terraform/dependencies](https://learn.hashicorp.com/tutorials/terraform/aws=dependency)


<h3 style='color:yellowgreen'>three provider in a question </h3>
look at the question below. and where will the resource be provisioned ?

<img src="../img/terraform resource.png" width=600 heigh=600>

the resource above will be created in the default region of us-east-1. since the resource does signify and alternative rpovider configuration. if the resource needs to be created in one of the other declared regions. it hsould have looked like this, where aws signifies the provider name and "west" signifies the alias name as such

```
    resource "aws_instance" "vault"{
        provider = aws.west
        instance_type= "t3.micro"

        and so on !!
    }

```

<h3 style='color:yellowgreen'>Easy-Peasy</h3>

During a Terraform apply, any resources that are successfully provisioned are maintained as `deployed`. on the other hand resources that failed during the provisioning process such as provisioned will be `tainted` to be recreated during the next run. This

🌟 🌟 🌟 <h3 style='color:yellowgreen'>what if I remove the version of module and run terraform init again </h3>

<img src="../img/constraint.png">]

terraform would use the existing module already downloaded.