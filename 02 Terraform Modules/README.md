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
