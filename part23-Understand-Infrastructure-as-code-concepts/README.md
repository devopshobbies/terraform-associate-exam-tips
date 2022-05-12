

<h3 style='color:yellowgreen'>Terraform needs credentials</h3>
there are several different ways to configure credentials.

- integrated services, such as AWS IAM or Azure Managed Service Identity
- use environments variable
- directly in the provider block by hardcoding or using a variable

🌟🌟🌟 <h1 style='color:yellowgreen'>IaC benefits</h1>
If you are new to infrastructure as code as a concept, it is the process of managing infrastructure in a file or files rather than manually configuring resources in a user interface. A resource in this instance is any piece of infrastructure in a given environment, such as a virtual machine, security group, network interface, etc.

At a high level, Terraform allows operators to use HCL to author files containing definitions of their desired resources on almost any provider (AWS, GCP, GitHub, Docker, etc) and automates the creation of those resources at the time of application.

- provide configurations consistency and standardization among deployments
- gives the user the ability to recreate and applications infrastructure for disaster recovery scenarios
- allows a user to turn a manual task into a simple , automated deployment
- relatively simple to learn and write regardless of a users's prior experience with developing code 
- allowing the user to reuse code deploy similar , yet different resources
