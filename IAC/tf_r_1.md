
## An extensive intro to IAC and Terraform:

How can you define resources, like VMs / ec2's / droplets, vpc, subnets and everything in between, without resulting to tens or hundreds of clicks in your cloud provider's UI, instead defining it in code, applying it in need and destroying when not?

This article is pointed at new comers to the IT/ Operations world, or any one who's getting into provision infrastructure in a way that is easy to reproduce, make copies of, change [drastically, just a bit or even delete entirely], minimise errors, and collaborate on with co-ops.

Terraform is the leading platform for cloud (but not only) IAC - Infrastructure As Code which basically mean its a tool to define and automate resources provisioning in the cloud or any data center it may be. 

### Some other popular tools with the same goal, to define infra through code are: 

- Pulumi -> a universal infrastructure as code platform, tames the cloud's complexity using the world's most popular programming languages.
- AWS CloudFormation -> model, provision, and manage AWS and third-party resources with IAC.
- Crossplane -> enables you to orchestrate applications and infrastructure no matter where they run, with a highly configurable frontend that lets you define the declarative API it offers, works with k8s api, trough yaml manifests and CRDs.

check out v. farcic video about the different tools, really an incredible video.
See the github repo where i wrote some code for the purpose of studding Terraform (, Pulumi & Crossplane)

### What is Terraform?

To get started i'd refer you to some better written and by more experienced - articles, which I highly recommend reading [ out loud if possible :) ] ->

- Start with: resources, variables, providers, outputs, for-each, count and conditions.
- Terraform modularity: about repeatable code structure and organising by folders.
- State: about state mechanism; what, why, how and limits.
- Tips & Tricks: loops, if-statements, and gotchas.
- Some more : Official Docs, JHOOK, Bits Lovers', Input Validation, Importing
- Finally, look this up: Terraform Cloud, Terraform Enterprise, Terraform Provisioners, Terraform Registry, Terraform Versions.
---

## Geting started:

### Getting the source files:

Clone [this repo][tf-repo] to a local folder, changing directory (cd) to the first example ready to use:

```bash
    git clone https://github.com/Slvr-one/IAC_Dev tf_training
    cd tf_training/Terraform/Ready/1
    tree
```
### overview:

This example uses the aws provider with Terraform, meaning terraform uses the AWS API to provision resources of AWS (such as vpc, ec2 vm's, s3 buckets, etc.).
Feel free to look around, youll notice a file structure that seperats to diffrent categories, files & directories like so:

```bash
.
├── modules
│   ├── compute
│   │   ├── instances.tf
│   │   ├── lb.tf
│   │   ├── outputs.tf
│   │   ├── template.tf
│   │   ├── user_data
│   │   │   └── nginx.tftpl
│   │   └── vars.tf
│   ├── network
│   │   ├── outputs.tf
│   │   ├── sg.tf
│   │   ├── subnets.tf
│   │   ├── vars.tf
│   │   └── vpc.tf
│   └── tls
│       ├── aws_keypair.tf
│       ├── outputs.tf
│       ├── private_key.tf
│       └── variables.tf
├── data.tf
├── dev.auto.tfvars
├── main.tf
├── outputs.tf
├── providers.tf
└── vars.tf
```

A standard procedure with Terraform, is that all files with .tf extensions are propagated to a single large one with project initiatoion, it doesn't matter how many files you separate your code into, so might as well organise by resources.

Directories also form independent of each other, to create modularity of code, so its possible to call a module (a dir of .tf files) more than once, with different values injected into it, from the root directory's main.tf file which call those modules by relative path. 

### Seperation to files:
- providers.tf -> defines where to install providers from, which providers and their individual configuration.
- variables.tf -> defines variables that can be used in the project, their type (bool / string / etc.), their description (optional but recomended, about the variable), you can also provide a default value to each variable but I normaly sway from that option, not from laziness, granted - I am; but to stay sharp on those by sending values with a .tfvars and not leave them to default.
- outputs.tf -> defines outputs that terraform output when project is done initializing, or been applied. they are essentialy parameters of resources you want to access, or know by value, you can also always access those with terraform subcommand `output`.
- main.tf -> defines the project's main file, which calls all modules you define and want to take part in.
- dev.auto.tfvars -> defines values to variables defined in variables.tf, the `.auto` means `terraform apply` will detect the file and inject those values automatically, with no need to specify `--var-file`.

### Seperation to folders (modules)

- network module: for the vpc, subnets, security groups & protocols
- compute: for ec2 instances, load balancing
- storage: s3 buckets and associated resources
- iam: iam roles, rules, assosations

- helm: helm provider, helm releases ans repos
- tls: tls provider, private key generation
- external: external provider, external programing options

### Provisioning resources:

To use your IAM credentials to authenticate the Terraform AWS provider, you can set the AWS_ACCESS_KEY_ID  & AWS_SECRET_ACCESS_KEY environment variablesand get going.
Though a more secure and long lasting way is to use a [credential file][config-files] which terraform automatically looks for when dealing with AWS provider and api. you can use the example file in the aformentioned repo [make sure to replace with your Private-KEY & KEY-ID] then locate the file in `~/.aws/credentials`. you can easly achive all that by running `aws configure` and inserting by the prompt, credentials and the rest will be saved to a corresponding files in `~/.aws`.

## summury:

terraform is defenetly a big step forward from dealing with web UIs for infra provisioning, its not too hard to grasp and extensivly documented, oficially and otherwise. by working on this project, though ive only just started to, I already proven to myself you can always finetune your code, always make it cleaner and nicer to read, you start somewhere, and you only move forward.

[aws-account]: https://aws.amazon.com/
[tf-repo]: https://github.com/Slvr-one/IAC_Dev 
[config-files]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html