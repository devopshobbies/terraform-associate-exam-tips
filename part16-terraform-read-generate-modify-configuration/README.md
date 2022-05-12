<h1 style='color:yellowgreen'>variable declarations</h1>
if it says type is list so the default is going to use [],
if it says type is map so the default is going to use {},
type= object({}) is correct , it's just empty 

<h1 style='color:yellowgreen'>terraform resource dependencies</h1>
 Terraform analyzes any expressions within a resource block to find references to other objects and treats those references as implicit ordering requirements when creating, updating, or destroying resources.

<h1 style='color:yellowgreen'>what is terraform list </h1>
 A terraform list is a sequence of values identified by consecutive whole numbers starting with zero.
 <h1 style='color:yellowgreen'>terraform console </h1>
the terraform console command provides an interactive console for evaluating expressions.

<h1 style='color:yellowgreen'>Index function</h1>

`index` finds the element index for a given value in a list .

`index(list, value)`

the returned index is zero-based. this function produces an error if the given value is not present in the list.

```
> index(["a", "b", "c"], "b")
1
```

<h1 style='color:yellowgreen'>min functions</h1>

How you can use min function ?
min takes one or more numbers and returns the smallest number from the set 
` min (12,54,3)` result is 3

if the numbers are in a list or set value, use ... to expand the collection to individual arguments:
`min ([12,54,3]...)` result is 3

<h1 style='color:yellowgreen'>Zipmap function</h1>

`zipmap` constructs a map from a list of keys and a corresponding list of values.

`zipmap(keyslist, valueslist)`

Both keyslist and valueslist must be of the same length. keyslist must be a list of strings, while valueslist caan be a list of any type.

```
> zipmap(["a", "b"], [1, 2])
{
  "a" = 1,
  "b" = 2,
}

``` 

<h1 style='color:yellowgreen'>Lookup function</h1>

`lookup` retrieves the value of a single element from a map, given its key.if the given key does not exist the given default value is returned instead.

`lookup(map, key, default)`
``` 

> lookup({a="ay", b="bee"}, "a", "what?")
ay
> lookup({a="ay", b="bee"}, "c", "what?")
what?

```


<h1 style='color:yellowgreen'>in what phase does terraform actually retrieve the data?</h1>
 It is important to consider that Terraform reads from data sources during the plan phase and writes the result into the plan. For something like a Vault token which has an explicit TTL, the apply must be run before the data, or token, in this case, expires, otherwise, Terraform will fail during the apply phase.
 
Another example of this is AWS credentials:

The token is generated from the moment the configuration retrieves the temporary AWS credentials (on terraform plan or terraform apply). If the apply run is confirmed after the 120 seconds, the run will fail because the credentials used to initialize the Terraform AWS provider has expired. For these instances or large multi-resource configurations, you may need to adjust the default_lease_ttl_seconds.


<h1 style='color:yellowgreen'>what is the easiest way for terroform to read and write secrets from Vault?</h1>
The Vault provider allows Terraform to read from, write to, and configure Hashicorp Vault.



<h1 style='color:yellowgreen'>what is downside to using a terraform provider such as Vault Provider</h1> Interacting with Vault from Terraform causes any secrets that you read and write to be persisted in both Terraform's state file and in any generated plan files. For any Terraform module that reads or writes Vault secrets, these files should be treated as sensitive and protected accordingly.

<h1 style='color:yellowgreen'>terraform and vault</h1>
You do not need to specify every required argument in the backend configuration. Omitting certain arguments may be desirable to avoid storing secrets, such as access keys, within the main configuration. When some or all of the arguments are omitted, we call this a partial configuration.

With a partial configuration, the remaining configuration arguments must be provided as part of the initialization process. There are several ways to supply the remaining arguments:

Interactively: Terraform will interactively ask you for the required values unless interactive input is disabled. Terraform will not prompt for optional values.

File: A configuration file may be specified via the init command line. To specify a file, use the -backend-config=PATH option when running terraform init. If the file contains secrets it may be kept in a secure data store, such as Vault, in which case it must be downloaded to the local disk before running Terraform.

Command-line key/value pairs: Key/value pairs can be specified via the init command line. Note that many shells retain command-line flags in a history file, so this isn't recommended for secrets. To specify a single key/value pair, use the -backend-config="KEY=VALUE" option when running terraform init.

🌟🌟🌟  <h1 style='color:yellowgreen'>join function</h1>

`join` produces a string by concatenating together all elements of a given list of strings with the given delimiter.
`join(separator, list)`

```
> join(", ", ["foo", "bar", "baz"])
foo, bar, baz
> join(", ", ["foo"])
foo
```