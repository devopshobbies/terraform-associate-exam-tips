<h1 style='color:yellowgreen'>TF_LOG</h1>
 Debugging Terraform
 To persist logged output you can set TF_LOG_PATH in order to force the log to always be appended to a specific file when logging is enabled. Note that even when TF_LOG_PATH is set, TF_LOG must be set in order for any logging to be enabled.
 You can set TF_LOG to one of the log levels TRACE, DEBUG, INFO, WARN or ERROR to change the verbosity of the logs. TRACE is the most verbose and it is the default if TF_LOG is set to something other than a log level name.
 For example, to always write the detailed logs to the directory you're currently running terraform from:
```
 export TF_LOG=TRACE
 export TF_LOG_PATH=./terraform.log
 ```
 
<h1 style='color:yellowgreen'> data source </h1>
 It is important to consider that Terraform reads from data sources during the plan phase and writes the result into the plan. Thus while working with the providers like Vault where values have associated TTL, a subsequent apply will likely fail if it is run after the intermediate token has expired due to the revocation of the secrets stored in the plan.

<h1 style='color:yellowgreen'> Terraform Debug log  </h1>

To persist logged output you can set TF_LOG_PATH in order to force the log to always be appended to a specific file when logging is enabled. Note that even when `TF_LOG_PATH` is set, `TF_LOG` must be set in order for any logging to be enabled.
` export TF_LOG_PATH="/var/log/terraform.log"`
`TF_LOG appear on stderr`

<h1 style='color:yellowgreen'> unset TF_LOG </h1>

 To disable, either unset it or set it to empty.
```
 # Set it to empty
 export TF_LOG=
 OR
 # Unset it
 unset TF_LOG
```