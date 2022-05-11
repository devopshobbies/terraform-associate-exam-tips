
<h3 style='color:yellowgreen'>Terraform lock file</h3>
what happen if multiple user attempt to run a terraform apply simultaneously when using a remote backend ?

if the state is configured for remote state, the backend selected will determine what happends. if the backedn support locking, the file will be locked for the first user , and the users's configuration will be applied . the second users;'s terraform applyu will return an error that the state is locked. if the remote backend does not support locking, the state file could become corrupted, since multiple users are trying to make changes at the same time

[https://www.terraform.io/language/state/locking](https://www.terraform.io/language/state/locking)

<h3 style='color:yellowgreen'>Terraform sensitive information</h3>
when ever you use these three methods, username/password will being written to the state file.

- useing a declared variable
- retrieving the credentials from a data source, such as HashiCorp Vault
- using a tfvars file

🌟 Environment vaiables can end up in log files if TF_LOG is set to TRACE.




<h3 style='color:yellowgreen'>remove resource from state manually </h3>

if you remove a resource from state manually with the `terraform state rm vsphere_virtual_machine.app1` and run terraform apply. the Terraform was no longer aware of the virtual machine and it refreshed the state file and discovered that the configuration file declared a virtual machine but it was not in state, there fore Terraform needed to create a virtual machin so the provisioned infrastructure matched the desired configuration. which is the Terraform configuration file.


[watch this video ](https://www.youtube.com/watch?v=7gO42TuioHc)
