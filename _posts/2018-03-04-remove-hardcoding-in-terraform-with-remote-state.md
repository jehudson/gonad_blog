---
layout: post
title: "Remove hard-coding in Terraform by using remote state"
excerpt: "Terraform stores information about the infrastructure you are building in a local state file which is called terraform.tfstate."
categories: [Terraform]
comments: true

---

Terraform stores information about the infrastructure you are building in a local state file which is called terraform.tfstate. Terraform uses this local state to create plans and make changes to your infrastructure. If you want to examine the state file you can run the following commands:

```bash
terraform state pull
terraform state pull | grep "search term"
terraform state pull | less
```

The first command will give a complete list of the stored state in JSON format.
You can use the second command to search for occurances of a specific term.
The third command will allow you to go backwards and forwards in the state file and search for occurances of a specific term by prefixing the search term with /.

You will notice in the state file that there is a section titled "outputs". You can add your own values to the outputs section. As an example you might be using modules, you could create an output in a particular module and then refer to the value in another section of your code. 

Here is an example output.

```bash
output "pdswebops_vpc_peering_connection" {

  value = "${aws_vpc_peering_connection.pdswebops.id}"

}

```
If you run terraform plan and then run terraform state pull, you will see the output as follows: All the outputs you have created will show up as a list with their values at the end of the terraform plan or apply commands.

```bash
“outputs”: {

 

                    "pdswebops_vpc_peering_connection": {

                    "sensitive": false,

                    "type": "string",

                    "value": "pcx-bf3774f9"

                }

}
```
You can then refer to the output in another section of your terraform code by creating a data source:

```bash
vpc_peering_connection_id =  “${data.pdswebops_peering_correction}”
```

Pulling data out of your local state file ceases to become practical if you are working in teams because you will need to save the state file somewhere, e.g. to a git repository so a team member can access the information. Also if you are working with more than one account, the local state file will only contain information for the account that you are working in. In this case you will need to create a remote state file that will allow simultaneous access.

Terraform supports remote state in a number of ways however if you are working with AWS, then S3 is most convenient. You can create a remote state file in an S3 bucket with a command similar to that shown below:

```bash
terraform {

  backend "s3" {

    bucket = "pdswebops.tfstate"

    key    = "path/to/my/key"

    region = "eu-west-1"

  }

}
```
More information about creating Terraform remote state can be found at <https://www.terraform.io/docs/backends/types/s3.html>

Once you have created a remote state file in AWS S3 you can refer to a value like the expression below:

```bash
data "terraform_remote_state" "pdswebops_vpc_core" {

  backend = "s3"

 

  config {

    bucket = "pdswebops.tfstate"

    key    = "vpc_core"

    region = "${var.aws_region}"

  }

}


vpc_peering_connection_id =  “$data.terraform_remote_state.pdswebops_vpc_core.pdswebops_peering_correction}”
```

The account accessing the remote state bucket must be given permisson using a bucket permissions policy, see the previous link.

If we create outputs for  our values and put them into remote state files, we can access them as variables rather than hard-coding subnets, route table, peering connection ids, etc. In this way when you make large changes in your infrastructure you will not need to go through your code changing the hard-coded data because your new data will be stored at run time and passed as variables.
  
