<h1 style='color:yellowgreen'>How IaC helps </h1>

IaC Makes Infrastructure More Reliable
IaC makes changes idempotent, consistent, repeatable, and predictable. Without IaC, scaling up infrastructure to meet increased demand may require an operator to remotely connect to each machine and then manually provision and configure many servers by executing a series of commands/scripts. They might open multiple sessions and move between screens, which often results in skipped steps or slight variations between how work is completed, necessitating rollbacks. Perhaps a command was run incorrectly on one instance and reverted before being re-run correctly.
These process inconsistencies can result in slight differences between servers that compound over time and could impact their performance, usability, or security. If a large team is applying changes, the risks increase because individuals don’t always follow the same instructions identically.
With IaC, we can test the code and review the results before the code is applied to our target environments. Should a result not align to our expectations, we iterate on the code until the results pass our tests and align to our expectations. Following this pattern allows for the outcome to be predicted before the code is applied to a production environment. Once ready for use, we can then apply that code via automation, at scale, ensuring consistency and repeatability in how it is applied.
Since code is checked into version control systems such as GitHub, GitLab, BitBucket, etc., it is possible to review how the infrastructure evolves over time. The idempotent characteristic provided by IaC tools ensures that, even if the same code is applied multiple times, the result remains the same.

<h1 style='color:yellowgreen'>advantages of IaC </h1>
 Infrastructure as Code
 Infrastructure as Code is the process of managing infrastructure in a file or files rather than manually configuring resources in a user interface. A resource in this instance is any piece of infrastructure in a given environment, such as a virtual machine, security group, network interface, etc.
 Infrastructure as Code (IaC) is the management of infrastructure (networks, virtual machines, load balancers, and connection topology) in a descriptive model, using the same versioning as DevOps team uses for source code. Like the principle that the same source code generates the same binary, an IaC model generates the same environment every time it is applied. IaC is a key DevOps practice and is used in conjunction with Continuous Delivery.
 At a high level, Terraform allows operators to use HCL to author files containing definitions of their desired resources on almost any provider (AWS, GCP, GitHub, Docker, etc) and automates the creation of those resources at the time of apply.
 Benefits of infrastructure as code
 Infrastructure as code brings a lot of benefits -
 Visibility: An infrastructure as code template serves as a very clear reference of what resources are on your account, and what their settings are. You don’t have to navigate to the web console to check the parameters.
 Stability: If you accidentally change the wrong setting or delete the wrong resource in the web console you can break things. Infrastructure as code helps solve this, especially when it is combined with version control, such as Git.
 Scalability: With infrastructure as code you can write it once and then reuse it many times. This means that one well-written template can be used as the basis for multiple services, in multiple regions around the world, making it much easier to horizontally scale.
 Security: Once again infrastructure as code gives you a unified template for how to deploy your architecture. If you create one well-secured architecture you can reuse it multiple times, and know that each deployed version is following the same settings.
 Transactional: Terraform not only creates resources on your cloud account but also waits for them to stabilize while they start. It verifies that provisioning was successful, and if there is a failure it throws an error and try to recreate resource again during next apply.

🌟🌟🌟 <h1 style='color:yellowgreen'>best describes the primary use of Infrastructure as Code (IaC)</h1>
The primary use case for IaC is to deploy and configure resources in almost any environment in a single, unified way that also abstracts the user from the APIs.
the ability to programmatically deploy and configure resources