<h1 style='color:yellowgreen'>provider dependencies </h1>

Each time a new provider is added to configuration -- either explicitly via a provider block or by adding a resource from that provider -- Terraform must` initialize` the provider before it can be used. Initialization downloads and installs the provider's plugin so that it can later be executed.
Before a new provider can be used, it must be `initialized` and `declared or used ini a configuration file `

<h1 style='color:yellowgreen'>Terraform plugin</h1>
Terraform is built on a plugin-based architecture. All providers and provisioners that are used in Terraform configurations are plugins, even the core types such as AWS and Heroku. Users of Terraform are able to write new plugins in order to support new functionality in Terraform. Terraform providers are terraform plugin

<h3 style='color:yellowgreen'>TF_LOG</h3>
TF_LOG levels are TRACE,DEBUG,INFO,WARN,ERROR,
TRACE is the most verboseand it is the default if TF_LOG is set to something other than a log level name.
to turning it of use TF_LOG=""

<h3 style='color:yellowgreen'>Hashicorp style</h3>

when writing Terraform code Hashicorp recommends that you use `2` spaces between each nesting level