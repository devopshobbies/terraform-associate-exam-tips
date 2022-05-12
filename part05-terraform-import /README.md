<h1 style='color:yellowgreen'>Terraform import support</h1>
 It is true, not all providers and resources support Terraform import.<br>

<h1 style='color:yellowgreen'>Terraform import </h1>
 Import Usage
 To import a resource, first write a resource block for it in your configuration, establishing the name by which it will be known to Terraform:
 resource "aws_instance" "quiz_experts_vm" {
     # ...instance configuration...
 }
 The name "quiz_experts_vm" here is local to the module where it is declared and is chosen by the configuration author. This is distinct from any ID issued by the remote system, which may change over time while the resource name remains constant.
 If desired, you can leave the body of the resource block blank for now and return to fill it in once the instance is imported.
 Now terraform import can be run to attach an existing instance to this resource configuration:

` terraform import aws_instance.quiz_experts_vm i-c4gd1563l`

This command locates the AWS instance with ID i-c4gd1563l. Then it attaches the existing settings of the instance, as described by the EC2 API, to the name aws_instance.quiz_experts_vm of a module. In this example, the module path implies that the root module is used. Finally, the mapping is saved in the Terraform state.

` The import command can import resources into modules as well as directly into the root of your state.`


🌟🌟🌟 <h1 style='color:yellowgreen'>where should you store the credentials rather than storing them in plaintext </h1>

- environment variables 
- credentials file

Explanation
Some backends allow providing access credentials directly as part of the configuration for use in unusual situations, for pragmatic reasons. However, in normal use, HashiCorp does not recommend including access credentials as part of the backend configuration. Instead, leave those arguments completely unset and provide credentials via the credentials files or environment variables that are conventional for the target system, as described in the documentation for each backend.