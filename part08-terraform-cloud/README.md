<h1 style='color:yellowgreen'>terraform Sentinel </h1>
 Sentinel is an embedded policy-as-code framework integrated with the HashiCorp Enterprise products. It enables fine-grained, logic-based policy decisions, and can be extended to use information from external sources.
 Sentinel can enforce compliance policies and policies are checked when a run is performed, after the terraform plan but before it can be confirmed or the terraform apply is executed.

 <h1 style='color:yellowgreen'>terraform workspaces in cloud </h1>
  Terraform Cloud and Terraform CLI both have features called "workspaces," but they're slightly different. CLI workspaces are alternate state files in the same working directory; they're a convenience feature for using one configuration to manage multiple similar groups of resources.
  In addition to the basic Terraform content, Terraform Cloud keeps some additional data for each workspace:
  State versions: Each workspace retains backups of its previous state files. Although only the current state is necessary for managing resources, the state history can be useful for tracking changes over time or recovering from problems.
  Run history: When Terraform Cloud manages a workspace's Terraform runs, it retains a record of all run activity, including summaries, logs, a reference to the changes that caused the run, and user comments.
  
   <h1 style='color:yellowgreen'>terraform Enterprise features only</h1>
   Terraform Enterprise is our self-hosted distribution of Terraform Cloud. It offers enterprises a private instance of the Terraform Cloud application, with no resource limits and with additional enterprise-grade architectural features like audit logging and SAML single sign-on.

   <h1 style='color:yellowgreen'>terraform cloud</h1> Terraform Cloud
   Terraform Cloud is an application that helps teams use Terraform together. It manages Terraform runs in a consistent and reliable environment, and includes easy access to shared state and secret data, access controls for approving changes to infrastructure, a private registry for sharing Terraform modules, detailed policy controls for governing the contents of Terraform configurations, and more.
   Terraform Cloud always encrypts state at rest and protects it with TLS in transit. Terraform Cloud also knows the identity of the user requesting state and maintains a history of state changes. This can be used to control access and track activity. Terraform Enterprise also supports detailed audit logging.

   <h1 style='color:yellowgreen'>sensitive data in state</h1>
    Sensitive Data in State
    Terraform state can contain sensitive data, depending on the resources in use and your definition of "sensitive." The state contains resource IDs and all resource attributes. For resources such as databases, this may contain initial passwords.
    When using a local state, the state is stored in plain-text JSON files.
    Recommendations
    If you manage any sensitive data with Terraform (like database passwords, user passwords, or private keys), treat the state itself as sensitive data.
    Storing the state remotely can provide better security.
