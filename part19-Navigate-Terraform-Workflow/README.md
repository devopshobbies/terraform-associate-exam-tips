<h1 style='color:yellowgreen'>how make sensitive information being not written to the state file </h1>
 The only method list above that will not result in the username/password being written to the state file is environment variables. All of the other options will result in the provider's credentials in the state file.
 
Terraform runs will receive the full text of sensitive variables, and might print the value in logs and state files if the configuration pipes the value through to an output or a resource parameter. Additionally, Sentinel mocks downloaded from runs will contain the sensitive values of Terraform (but not environment) variables. Take care when writing your configurations to avoid unnecessary credential disclosure. Whenever possible, use environment variables since these cannot end up in state files or in Sentinel mocks. (Environment variables can end up in log files if TF_LOG is set to TRACE.)

<a href="https://www.terraform.io/docs/cloud/workspaces/variables.html#sensitive-values" target="_blank">https://www.terraform.io/docs/cloud/workspaces/variables.html#sensitive-values</a>

<a href="https://learn.hashicorp.com/tutorials/terraform/sensitive-variables" target="_blank">https://learn.hashicorp.com/tutorials/terraform/sensitive-variables</a>


<h1 style='color:yellowgreen'>commands to detect configuration drift</h1>
If the state has drifted from the last time Terraform ran, refresh allows that drift to be detected.



<h1 style='color:yellowgreen'>Terraform OSS store the local state </h1>
For the local state, Terraform stores the workspace states in a directory called terraform.tfstate.d

<h1 style='color:yellowgreen'>Terraform backend list </h1>
- local
- remote
- artifactory
- azurerm
- consul
- cos
- etcd
- etcdv3
- gcs
- http
- Kubernetes
- manta 
- OSS
- pg 
- s3
- swift
<h1 style='color:yellowgreen'>Terraform unlock state </h1>
terraform force-unlock removes the lock on the state for the current configuration. Be very careful forcing an unlock, as it could cause data corruption and problems with your state file.