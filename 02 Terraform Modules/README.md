<h3 style='color:yellowgreen'>version</h3>
when using modules installed from a module registry , Terraform recommend explicity constraining the acceptable verson numbers to avoid unexpected ro unwanted changes.
version meta-arguments is not mandatory but it is highly recommended to avoid pulling breaking changes .

modules sourced from local file paths do not support version; since they're loaded from the same source repository , they always share the same version as their caller.

module can be public or private.

the module installer supports installation from a number of different source types as listed below
 
 - local paths
 - Terraform registry 
 - Github/bitbucket
 - Generic Git, Mercurial repositories,
 - HTTP URLs
 - S3 buckets
 - GCS buckets


<h3 style='color:yellowgreen'>S3 archive</h3>

you can use archives stored in S3 as module sources using the special s3:: prefix. the s3:: prefix causes Terraform to use AWS-style authentication when accessing the given URL.

the module installer looks for AWS credentials in the following locations, preferring those earlier in the list when multiple are available
- The AWS_ACCESS_KEY_ID , AWS_SECRET_ACCESS_KEY environment variables,
- the default profile in the .aws/credentials file in your home directory
- if running on an eec2 instance , temprorary crendetials associated with instance's IAM instance profile.

<h3 style='color:yellowgreen'>Multiple instances of a Module</h3>

Module support for the for_each and count meta-arguments was added in Terraform 0.13 previous version can only use these argumnets with individual resources.

<h3 style='color:yellowgreen'>variable for modules</h3>
whenever you use modules you need to provide input variables values to the child module explicitly in the module block. 

<h3 style='color:yellowgreen'>How upgrade modules</h3>
by default terraform init command will not upgrade and already-installed module; use the -upgrade option to instead upgrade to the newest available version.

<h3 style='color:yellowgreen'>taint</h3>
if you are tainting a nested module be aware that you have to call the module even it is a nested 

`terraform taint module.test_master_cluster.module.instance.test`

I've taint an vm named test in the instance module under a test_master_cluster module

<h3 style='color:yellowgreen'>source</h3>

Terraform uses this during the module installation step of terraform init to download the source code to a directory on local disk so that it can be used by other Terraform commands.

<h3 style='color:yellowgreen'>additional provider</h3>
additional provider configurations (thise with the aliase argument set) are never inherited automatically but child modules , and so must always be passed explicitly using the providers map. 

<h3 style='color:yellowgreen'>How select local Paths?</h3>
Local path refrences allow for factoring out portions of a configuration within a single source repository

a local path must begin with either ./ or ../ to indicate that a local path is intended, to distinguish from a module registry address.


<h3 style='color:yellowgreen'>get module from specific branch on git</h3>
By default, Terraform will clone and use the default branch (refreneced by HEAD) in the selected repository. you can override this using the ref argument:
 to specify a tag

 ```
    module "vpc" {
        source = "git::https://code.quizeexperts.com/vpc.git?ref=v1.2.0"
    }

    # to specify a Branch 
    module "vpc" {
        source = "git:https://code.babak.com/vpc.git?ref=hotfix"
    }

 ```

 <h3 style='color:yellowgreen'> Module and Providers</h3>

 Providers can be passed down to descendent modules in two ways:
 
 - implicitly through inheritance
 - explicitly via the Providers argument within a module block.

Provider configurations can be defined only in a root Terraform module.
A module intended to be called by one or more other modules must not contain any provider blocks, with the exception of the special "proxy provider blocks"

additional Provider configurations  (those with the alias argument set)  are never inherited automatically by child modules, and so must always be passed explicitly using the providers map.

<h3 style='color:yellowgreen'>at least one module?</h3>
Every Terraform configuration has at least one module, known as its root module,which consists of the resources defined in the .tf files in the main working directory.


<h3 style='color:yellowgreen'>module attributes</h3>
the resources defined in the module are encapsulated so the calling module cannot access their attributes directly. However, the child module can declare output values to selectively export certain values to be accessed by the calling module.
 For example , if the ./app-cluster module referenced in the example about exported an output value named instance_ids then the calling module can reference the result using the expression module.instances.instance_ids
 
 <h3 style='color:yellowgreen'>how use arbitrary git repositories?</h3>
 arbitrary git repositories can be used by prefixing the address with special git:: prefix after this prefix any valid git URL can be specified to select one of the protocols supported by git. For
 For example to use HTTPS or SSH: 

```
 module "vpc" { 
     source = "git::https://babak.com/vpc.git"
 }
 module "storage"{
     source = "git::ssh://username@example.com/storage.git"
 }
 ```