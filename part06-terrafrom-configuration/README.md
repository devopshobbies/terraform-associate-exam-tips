<h1 style='color:yellowgreen'>Terraform string type</h1>
 The Terraform language will automatically convert number and bool values to string values when needed, and vice-versa as long as the string contains a valid representation of a number or boolean value.
     *  true converts to "true", and vice-versa
     *  false converts to "false", and vice-versa
     *  15 converts to "15", and vice-versa

<h1 style='color:yellowgreen'>use dynamic variables for repeating inputs </h1>
Dynamic Blocks
Dynamic Blocks allow us to dynamically construct repeatable nested blocks.
A dynamic block acts much like a for loop.
Here is how we can modify Terraform configuration given in the question -

```
variable "ingress_ports" {
    type        = list(number)
  description = "list of ingress ports"
  default     = [443, 80, 8080, 8000]
}
resource "aws_vpc" "quiz_experts" {
    cidr_block = "172.16.0.0/16"
  tags = {
      Name = "Quiz Experts Demo"
  }
}
resource "aws_security_group" "allow_access_demo" {
    name        = "allow_tls"
  description = "Allow TLS inbound traffic"
  vpc_id      = aws_vpc.quiz_experts.id
  dynamic "ingress" {
      iterator = port
    for_each = var.ingress_ports
    content {
        from_port   = port.value
      to_port     = port.value
      protocol    = "tcp"
      cidr_blocks = [aws_vpc.quiz_experts.cidr_block]
    }
  }
  egress {
      from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  tags = {
      Name = "Quiz Experts"
    Task = "Demo"
  }
}
```

Best Practices for dynamic Blocks
Overuse of dynamic blocks can make configuration hard to read and maintain, so we recommend using them only when you need to hide details in order to build a clean user interface for a re-usable module. Always write nested blocks out literally where possible.

<h1 style='color:yellowgreen'>statement about providers</h1>

Providers Within Modules
In a configuration with multiple modules, there are some special considerations for how resources are associated with provider configurations.
Each resource in the configuration must be associated with one provider configuration. Provider configurations, unlike most other concepts in Terraform, are global to an entire Terraform configuration and can be shared across module boundaries. Provider configurations `can be defined only in a root` Terraform module.
Providers can be passed down to descendent modules in two ways: `either implicitly through inheritance`, or `explicitly via the providers argument within a module block`.
Provider Version Constraints in Modules
Although `provider configurations are shared between modules`, `each module must declare its own provider requirements`, so that Terraform can ensure that there is a single version of the provider that is compatible with all modules in the configuration and to specify the source address that serves as the global (module-agnostic) identifier for a provider.
To declare that a module requires particular versions of a specific provider, use a required_providers block inside a terraform block:

```
 terraform {
     required_providers {
       aws = {
         source  = "hashicorp/aws"
       version = "&gt;= 2.7.0"
     }
   }
 }
```

A provider requirement says, for example, "This module requires version v2.7.0 of the provider hashicorp/aws and will refer to it as aws." It doesn't, however, specify any of the configuration settings that determine what remote endpoints the provider will access, such as an AWS region; configuration settings come from provider configurations, and a particular overall Terraform configuration can potentially have several different configurations for the same provider.

 <h1 style='color:yellowgreen'>terraform CLI ARGS</h1>

`TF_CLI_ARGS` and `TF_CLI_ARGS_name`
The value of `TF_CLI_ARGS` will specify additional arguments to the command-line. This allows easier automation in CI environments as well as modifying the default behavior of Terraform on your own system.
These arguments are inserted directly after the subcommand (such as plan) and before any flags specified directly on the command-line. This behavior ensures that flags on the command-line take precedence over environment variables.
For example, the following command:`TF_CLI_ARGS="-input=false"`terraform apply -force is the equivalent to manually typing:` terraform apply -input=false -force`.
The flag `TF_CLI_ARGS` affects all Terraform commands. If you specify a named command in the form of `TF_CLI_ARGS_name` then it will only affect that command. As an example, to specify that only plans never refresh, you can set `TF_CLI_ARGS_plan="-refresh=false"`.
The value of the flag is parsed as if you typed it directly to the shell. Double and single quotes are allowed to capture strings and arguments will be separated by spaces otherwise.

 <h1 style='color:yellowgreen'>  Variable Definition Precedence</h1>
  Variable Definition Precedence
  If the same variable is assigned multiple values, Terraform uses the last value it finds, overriding any previous values. Note that the same variable cannot be assigned multiple values within a single source.
  Terraform loads variables in the following order, with later sources taking precedence over earlier ones:
         *  Environment variables
         *  The terraform.tfvars file, if present.
         *  The terraform.tfvars.json file, if present.
         *  Any *.auto.tfvars or *.auto.tfvars.json files, processed in lexical order of their filenames.
         *  Any -var and -var-file options on the command line, in the order they are provided.
            (This includes variables set by a Terraform Cloud workspace.)
  
 <h1 style='color:yellowgreen'>Expression a local value refer to other locals</h1>

The expression of a local value can refer to other locals, but as usual reference cycles are not allowed. That is, a local cannot refer to itself or to a variable that refers (directly or indirectly) back to it.
Example:

```
 locals {
     # Common tags to be assigned to all resources
   common_tags = {
       Service = local.service_name
     Owner   = local.owner
   }
 }
 resource "aws_instance" "example" {
     # ...
   tags = local.common_tags
 }
```

 <h1 style='color:yellowgreen'>List type tips</h1>

` List (Collection Type)`
A list is a sequence of values of the same type identified by consecutive whole numbers starting with zero.
For example, the type list(string) means "list of strings", which is a different type than list(number), a list of numbers. All elements of a collection must always be of the same type.
Whenever possible, Terraform converts values between similar kinds of complex types (list/tuple/set and map/object) if the provided value is not the exact type requested, therefore -
_ A tuple can be converted to a list, and vice-versa.
_ An object can be converted to a map and vice-versa.
However, certain conditions should meet to enable this conversion. For examples -
Example 1:
A list can only be converted to a tuple if it has exactly the required number of elements.
Example 2:
If a module argument requires a value of type list(string) and a user provides the tuple ["a", 15, true], Terraform will internally transform the value to ["a", "15", "true"] by converting the elements to the required string element type as all of the elements of a collection must have the same type.
On the other hand, an automatic conversion will fail if the provided value (including any of its element values) is incompatible with the required type. If an argument requires a type of list(string) and a user provides the tuple ["a", [], "b"] then the value cannot conform to the type constraint, Terraform would reject this value, complaining that all elements must have the same type, since an empty tuple cannot be converted to a string.
NOTE: The keyword list is a shorthand for list(any), which accepts any element type as long as every element is the same type. This is for compatibility with older configurations; for new code, we recommend using the full form.

 <h1 style='color:yellowgreen'>terraform primitive types</h1>
 
Primitive Types
A primitive type is a simple type that isn't made from any other type. All primitive types in Terraform are represented by a type keyword. The available primitive types are:
   *  `string`: a sequence of Unicode characters representing some text, such as "hello".
   *  `number`: a numeric value. The number type can represent both whole numbers like 15 and fractional values such as 6.283185.
   *  `bool`: either true or false. bool values can be used in conditional logic.


 <h1 style='color:yellowgreen'>terraform structural types </h1>
 
 Structural Types
 A structural type allows multiple values of several distinct types to be grouped together as a single value. Structural types require a schema as an argument, to specify which types are allowed for which elements.
 The two kinds of structural type in the Terraform language are:
    * object(...): A collection of named attributes that each have their own type.
 The schema for object types is { <key> = <type>, <key> = <type>, ... } — a pair of curly braces containing a comma-separated series of <key> = <type> pairs. Values that match the object type must contain all of the specified keys, and the value for each key must match its specified type. (Values with additional keys can still match an object type, but the extra attributes are discarded during type conversion.)   
      For Example: An object type of object({ name=string, age=number }) would match a value like the following:
 {
     name = "John"
   age  = 52
 }
    *  tuple(...): A sequence of elements identified by consecutive whole numbers starting with zero, where each element has its own type.
 The schema for tuple types is [<type>, <type>, ...] — a pair of square brackets containing a comma-separated series of types. Values that match the tuple type must have exactly the same number of elements (no more and no fewer), and the value in each position must match the specified type for that position.
       For Example: A tuple type of tuple([string, number, bool]) would match a value like the following:
 ["a", 15, true]
 
        
<h3 style='color:yellowgreen'>Local in Terraform</h3>
A local Value assings a name to an expression, so you can use it multiple times within a module without repeating it.

```
locals {
  service_name = "forum"
  owner        = "Community Team"
}

locals {
  # Ids for multiple sets of EC2 instances, merged together
  instance_ids = concat(aws_instance.blue.*.id, aws_instance.green.*.id)
}

locals {
  # Common tags to be assigned to all resources
  common_tags = {
    Service = local.service_name
    Owner   = local.owner
  }
}



resource "aws_instance" "example" {
  # ...

  tags = local.common_tags
}

```