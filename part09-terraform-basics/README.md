<h1 style='color:yellowgreen'>terraform Sentinel </h1>

` Disposable Environments`
 It is common practice to have both a production and staging or QA environment. These environments are smaller clones of their production counterpart, but are used to test new applications before releasing in production. Using Terraform, the production environment can be codified and then shared with staging, QA or dev. These configurations can be used to rapidly spin up new environments to test in, and then be easily disposed of. Terraform can help tame the difficulty of maintaining parallel environments, and makes it practical to elastically create and destroy them.
` Resource Schedulers`
 In large-scale infrastructures, static assignment of applications to machines becomes increasingly challenging. To solve that problem, there are a number of schedulers like Borg, Mesos, YARN, and Kubernetes. These can be used to dynamically schedule Docker containers, Hadoop, Spark, and many other software tools.
 Terraform is not limited to physical providers like AWS. Resource schedulers can be treated as a provider, enabling Terraform to request resources from them. This allows Terraform to be used in layers: to set up the physical infrastructure running the schedulers as well as provisioning onto the scheduled grid.
` Multi-Cloud Deployment`
 Terraform is cloud-agnostic and allows a single configuration to be used to manage multiple providers, and to even handle cross-cloud dependencies.
` Self-Service Clusters`
 At a certain organizational size, it becomes very challenging for a centralized operations team to manage a large and growing infrastructure. Instead, it becomes more attractive to make "self-serve" infrastructure, allowing product teams to manage their own infrastructure using tooling provided by the central operations team.
 Using Terraform, the knowledge of how to build and scale a service can be codified in a configuration. Terraform configurations can be shared within an organization enabling customer teams to use the configuration as a black box and use Terraform as a tool to manage their services.



<h3 style='color:yellowgreen'>Hashicorp Style</h3>
 HashiCorp style conventions suggest you that align the equals sign for consecutive arguments for easing readability for configurations
 
 ami="abc123"
 andinstance_type ="t2.micro"
 [https://www.terraform.io/language/syntax/style](https://www.terraform.io/language/syntax/style)

 <h3 style='color:yellowgreen'>Terraform style</h3>
Lists are defined with [] and maps are defined with {}
it means like this 

```
type = list(string)
default = [] not {}
```


<h3 style='color:yellowgreen'>retrieval of data</h3>
it is important to consider that Terraform reads from data sources during the plan phase and writes the result into the plan.For something like a Vault token wich has an explicit TTL the apply must be run before the data , or token, in this case, expires, otherwise, Terraform will fail during the apply phase.





<h3 style='color:yellowgreen'>Terraform depends on </h3>
Terraform analyzes any expressions within a resource block to find references to other objects and treats those references as implicit ordering requirements when creating, updating or destroying resources.


<h3 style='color:yellowgreen'>environment variable</h3>
Environment variables can be used to set variables. the environment variables must be in the format TF_VAR_name and this will be checked last for a value for example:

```
export TF_VAR_region=us-west-1
export TF_VAR_ami=ami-12312312
export TF_VAR_alist='[1,2,3]'
export TF_VAR_amap='{foo="bar",baz="qux"}'
```


🌟🌟🌟 <h1 style='color:yellowgreen'>terraform graph </h1>
The terraform graph command is used to generate a visual representation of either a configuration or execution plan. The output is in the DOT format, which can be used by GraphViz to generate charts.
`terraform graph | dot -Tsvg > graph.svg`

🌟🌟🌟 <h1 style='color:yellowgreen'>What Terraform command can be used to evaluate and experiment with expressions in your configuration</h1>

The` terraform console `command provides an interactive command-line console for `evaluating` and `experimenting with expressions`. This is useful for testing interpolations before using them in configurations, and for interacting with any values currently saved in state.

Example from the Terraform documentation:

`echo 'split(",", "foo,bar,baz")' | terraform console`

🌟🌟🌟 <h1 style='color:yellowgreen'>Which of the following are true regarding Terraform variables?</h1>
- When it comes to working with variables, the value that is used in the Terraform configuration will be stored in the state file, regardless of whether the sensitive argument was set to true. However, the value will not be shown in the CLI output if the value was to be exported by an output block.
- the default value will be found in the state file if no other value was set for the variable

🌟🌟🌟 <h1 style='color:yellowgreen'>terraform console</h1>
The terraform console command will read the Terraform configuration in the current working directory and the Terraform state file from the configured backend so that interpolations can be tested against both the values in the configuration and the state file.

When you execute a terraform console command, you'll get this output:
```
$terraform console
Acquiring state lock. this my take a few moments...
```