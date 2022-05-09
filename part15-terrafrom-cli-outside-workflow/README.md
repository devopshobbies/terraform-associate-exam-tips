<h1 style='color:yellowgreen'>terraform workspace</h1>
 Workspaces isolate their state, so if Pam runs a terraform destroy, Terraform will not see any existing state for this configuration. you may use the command terraform workspace select <name> to choose the original workspace where the Azure resources were provisioned in order to properly destroy them in your cloud.<br>
