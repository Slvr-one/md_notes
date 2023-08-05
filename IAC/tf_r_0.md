# State on s3:

Terraform uses state mechanisem to store its current configuration by code in a json format, and periodicely check it against th cloud provider to asses drifts and changes to the described reesources.

by default, the state file, and its lock is stored locally, in the root project directory, but that method has its drawbacks and its considered better practice to  save the state remotely, where other developers can access it (with suffisiont creds) and where iys safer from acedental deletion or humen errors of this sort.

I'd reecoomnd you start with this part, provision an s3 to save your future project's state in, remember that this particular provisioning state file will need to be saved locally, (unless you want to set a backend to a backend and so on.)

## File structure:
```bash
.
├── dev.auto.tfvars
├── main.tf
├── modules
│   ├── dynamo
│   │   ├── main.tf
│   │   └── outputs.tf
│   ├── kms
│   │   ├── main.tf
│   │   └── outputs.tf
│   └── s3
│       ├── main.tf
│       ├── outputs.tf
│       └── variables.tf
├── outputs.tf
├── providers.tf
└── variables.tf
```

## Overview:

* s3: Simple Storage Service by AWS Cloud, essentialy a bucket to store files in, similarly to a linux way at that, it'll store files needed to by terraform state procedure.
* dynamo: dynamodb is a dynamic structured key - value database in AWS Cloud, here we use a table and a lock mechanisem to lock the state to one person usage at a time.
* kms: the key managment service by AWS cloud, here providing the encyption key (and its periodic rotation) to the s3 bucket.