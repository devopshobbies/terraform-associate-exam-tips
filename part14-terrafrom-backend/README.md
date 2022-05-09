<h1 style='color:yellowgreen'>what if you delete remote backend from terraform</h1>
If you delete remote backend configuration from Terraform code and run terraform apply , it will detect backend change and throw an error. You need to reinitialize Terraform again, and it will prompt you to migrate existing remote state to the local backend so that it can still manage resources managed by the remote state.

<h1 style='color:yellowgreen'>terraform locking state</h1>
Image source - Internet
Why state locking?
Storing the state remotely lets you collaborate smoothly. It allows each of your team members to access the latest state files, but it also brings a risk of accidental state file corruption, especially when two team members are running Terraform at the same time you may run into race conditions as multiple terraform apply commands make concurrent updates to the same state files, leading to conflicts and data loss, and state file corruption.
State lock, a feature that prevents opening a state file while already in use, will resolve this problem. Please note that not all backends support locking. If your backend supports it and is enabled, Terraform will lock your state for all operations that could write state. This prevents others from acquiring the lock and potentially corrupting your state.
NOTE: Not all backends support locking. Please view the list of backend types for details on whether a backend supports locking or not.
You can implement the lock by creating an AWS DynamoDB Table.
Image source - Internet
To use DynamoDB for locking with Terraform, you must create a DynamoDB table with a primary key called LockID with this exact spelling and capitalization.
Here’s what the sample backend configuration looks like for an S3 backend with state lock:
```
terraform {
      backend "s3" {
        # Replace this with your bucket name.
    bucket         = "quiz-experts-terraform-bucket"
    key            = "global/quiz-experts/stats/terraform.tfstate"
    region         = "us-east-2"
    # Replace this with your DynamoDB table name.
    dynamodb_table = "quiz-experts-terraform-locks"
    encrypt        = true
  }
}
``` 
Remember to run terraform init again after making any change in the remote backend configuration. This command can not only download provider code, but also configure your Terraform backend. Moreover, the init command is idempotent, so it’s safe to run it over and over again.

