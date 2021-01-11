# Terraform Backend for AWS

This repository creates a terraform backend to store the state it needs to create a Client tenant.

The S3 bucket created applies various security policies to ensure the data stored within it is secure and only accessible to those granted access.

## Requirements

To use this repo you need the following tools installed

- git
- terraform > 0.14

## How to use

1. Clone this repository
2. Setup AWS authentication
3. Run Terraform
4. Save the generated file for use with other repos

Here is an example of these steps

```bash
$ git clone git@github.com:mojaloop/iac-aws-backend.git
$ cd iac-aws-backend
$ export AWS_ACCESS_KEY=*your_access_key_here*
$ export AWS_SECRET_ACCESS_KEY=*your_secret_here*
$ terraform init

Initializing the backend...

Initializing provider plugins...
...

$ terraform apply
var.client
  Name of client. Must not contain spaces

  Enter a value: acme

var.region
  AWS region

  Enter a value: eu-west-1

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create
 <= read (data resources)

Terraform will perform the following actions:
...

...
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

...

...
Apply complete! Resources: 6 added, 0 changed, 0 destroyed.

Outputs:

dynamodb_table_arn = arn:aws:dynamodb:eu-west-1:<AWS_ACCOUNT>:table/acme-mojaloop-lock
dynamodb_table_id = acme-mojaloop-lock
dynamodb_table_name = acme-mojaloop-lock
s3_bucket_arn = arn:aws:s3:::acme-mojaloop-state
s3_bucket_domain_name = acme-mojaloop-state.s3.amazonaws.com
s3_bucket_id = acme-mojaloop-state
terraform_backend_config = region         = "eu-west-1"
bucket         = "acme-mojaloop-state"
dynamodb_table = "acme-mojaloop-lock"
profile        = ""
role_arn       = ""
encrypt        = "true"

$ cat backend.hcl
region         = "eu-west-1"
bucket         = "acme-mojaloop-state"
dynamodb_table = "acme-mojaloop-lock"
profile        = ""
role_arn       = ""
encrypt        = "true"
```

Use the resultant `backend.hcl` with the [bootstrap](https://github.com/mojaloop/iac-aws-bootstrap) and [platform](mojaloop/iac-aws-platform) respositories.
