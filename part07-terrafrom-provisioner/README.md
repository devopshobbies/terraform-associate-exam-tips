<h1 style='color:yellowgreen'>provisioner and null resource </h1>
 PROVISIONERS WITH NULL_RESOURCE
 Provisioners can be defined only within a resource. Still, sometimes, you want to execute a provisioner without tying it to a specific resource. You can do this using something called the null_resource, which acts like a regular Terraform resource, except that it doesn't create anything. By defining provisioners on the null_resource, you can run your scripts as part of the Terraform life cycle without being attached to any "real" resource.
 resource "null_resource" "quiz-experts" {
     provisioner "local-exec" {
       command = "echo \"Hello, $(whoami)\""
   } 
 }
 The null_resource even has a useful argument termed triggers, which takes a map of keys and values. Whenever the values change, the null_resource will be recreated, forcing any provisioners within it to be re-executed. For instance, if you want to execute a provisioner within a null_resource every time you run terraform apply command, you can use the uuid() built-in function, which returns a new, randomly generated UUID each time.
 resource "null_resource" "quiz-experts" {
     triggers = {
       uuid = uuid()
   }
   provisioner "local-exec" {
       command = "echo \"Hello, $(whoami)\""
   } 
 }
 Reference: Book Terraform Up &amp; Running: Writing Infrastructure as Code
 
<h1 style='color:yellowgreen'>Failure behavior</h1>
Failure Behavior
By default, provisioners that fail will also cause the Terraform apply itself to fail. The on_failure setting can be used to change this. The allowed values are:
   * continue - Ignore the error and continue with creation or destruction.
   * fail - Raise an error and stop applying (the default behavior). If this is a creation provisioner, taint the resource.
Example:

```
resource "aws_instance" "quiz_experts" {
    # ...
  provisioner "local-exec" {
      command    = "Some command that will fail during apply"
    on_failure = continue
  }
}
```

<h1 style='color:yellowgreen'>Default provisioner ini a resource</h1>

By default, a provisioner defined in a resource is a creation-time provisioner.
If when = destroy is specified, the provisioner will run when the resource it is defined within is destroyed.
Creation-Time Provisioners
By default, provisioners run when the resource they are defined within is created. Creation-time provisioners are only run during creation, not during updating or any other lifecycle. They are meant as a means to perform bootstrapping of a system.
If a creation-time provisioner fails, the resource is marked as tainted. A tainted resource will be planned for destruction and recreation upon the next terraform apply. Terraform does this because a failed provisioner can leave a resource in a semi-configured state. Because Terraform cannot reason about what the provisioner does, the only way to ensure proper creation of a resource is to recreate it. This is tainting.
Destroy-Time Provisioners
If when = destroy is specified, the provisioner will run when the resource it is defined within is destroyed.

````resource "aws_instance" "web" {
     # ...
   provisioner "local-exec" {
       when    = destroy
     command = "echo 'Destroy-time provisioner'"
   }
 }```
 Destroy provisioners are run before the resource is destroyed. If they fail, Terraform will error and rerun the provisioners again on the next terraform apply. Due to this behavior, care should be taken for destroy provisioners to be safe to run multiple times.
 Destroy-time provisioners can only run if they remain in the configuration at the time a resource is destroyed.


                </div>
````

<h1 style='color:yellowgreen'>remote-exec provisioner</h1>

The remote-exec provisioner invokes a script on a remote resource after it is created. This can be used to run a configuration management tool, bootstrap into a cluster, etc. To invoke a local process, see the local-exec provisioner instead. The remote-exec provisioner supports both ssh and winrm type connections.

```
resource "aws_instance" "quiz_experts" {
    ami           = "ami-0450ea6e7c"
  instance_type = "t2.micro"
  connection {
      type        = "ssh"
    user        = "ec2-user"
    private_key = file("~/.ssh/terraform")
    host        = self.public_ip
  }
  provisioner "remote-exec" {
      inline = [
        "sudo amazon-linux-extras enable nginx1.12",
      "sudo yum -y install nginx",
      "sudo systemctl start nginx"
    ]
  }
}
```

NOTE: Provisioners should only be used as a last resort

<h1 style='color:yellowgreen'>destroy-time provisioner</h1>

A destroy-time provisioner within a resource that is tainted will not run. This includes resources that are marked tainted from a failed creation-time provisioner or tainted manually using terraform taint.

 <h1 style='color:yellowgreen'>creation-time provisioner</h1>
 If a creation-time provisioner fails, the resource is marked as tainted. A tainted resource will be planned for destruction and recreation upon the next terraform apply. Terraform does this because a failed provisioner can leave a resource in a semi-configured state. Because Terraform cannot reason about what the provisioner does, the only way to ensure proper creation of a resource is to recreate it. This is tainting.
 <h1 style='color:yellowgreen'>connection blocks</h1>
 
 Connection blocks don't take a block label, and can be nested within either a resource or a provisioner.
     * A connection block nested directly within a resource affects all of that resource's provisioners.
```
 resource "aws_instance" "quiz_experts" {
     ami           = "ami-04579a6a597"
   instance_type = "t2.micro"
   connection {
       type        = "ssh"
     user        = "ec2-user"
     private_key = file("~/.ssh/terraform")
     host        = self.public_ip
   }
   provisioner "remote-exec" {
       inline = [
         "sudo amazon-linux-extras enable nginx1.12",
       "sudo yum -y install nginx",
       "sudo systemctl start nginx"
     ]
   }
 }
```
    * A connection block nested in a provisioner block only affects that provisioner, and overrides any resource-level connection settings.

```
 resource "aws_instance" "quiz_experts" {
     # Something
   # Something
   # Copies the file as the root user using SSH
   provisioner "file" {
       source      = "conf/myapp.conf"
     destination = "/etc/myapp.conf"
     connection {
         type     = "ssh"
       user     = "root"
       password = "${var.root_password}"
       host     = "${var.host}"
     }
   }
 }
 ```
 Use case: One use case for providing multiple connections is to have an initial provisioner connect as the root user to set up user accounts, and have subsequent provisioners connect as a user with more limited permissions.
