---

# 🚀Deploying a 2-Tier Application with Terraform

✨ This repository is designed to help you learn and deploy a 2-tier application on AWS using Terraform.


### Create an S3 Bucket for Backend

First, create an S3 bucket to store the `.tfstate` file for Terraform's remote backend.

**Important!** It's highly recommended to enable **Bucket Versioning** on the S3 bucket. This ensures state recovery in case of accidental deletions or other issues.

**Note**: You’ll need the name of this S3 bucket for later steps.

### Set up a DynamoDB Table for State Locking

- Create a DynamoDB table with a name of your choice.
- Set the `Partition key` as `LockID` with type `String`.

### Generate SSH Key Pair for EC2 Instances

To generate a public and private key for your instances, use the following command:

```sh
cd modules/key/
ssh-keygen
```

This will generate a key pair (public and private) named `client_key`. You can customize the name, but you’ll need to update the Terraform configuration accordingly.

Edit the `root/backend.tf` file to match your setup:

```sh
vim root/backend.tf
```

Add the following configuration to `backend.tf`:

```hcl
terraform {
  backend "s3" {
    bucket = "YOUR_BUCKET_NAME"
    key    = "backend/STATE_FILE_NAME.tfstate"
    region = "us-east-1"
    dynamodb_table = "YOUR_DYNAMODB_TABLE_NAME"
  }
}
```

### 🏠 Configure Infrastructure Variables

Create a file named `terraform.tfvars`:

```sh
vim root/terraform.tfvars
```

### 🔐 Obtain ACM Certificate

Go to the AWS Console → **AWS Certificate Manager (ACM)**. Ensure you have a valid certificate with the status "Issued." If not, create a new certificate for the domain you plan to use for hosting your application.

### 👨‍💻 Set up Route 53 Hosted Zone

In the AWS Console, navigate to **Route 53** → **Hosted Zones**, and ensure you have a public hosted zone available. If not, create one.

Add the following variables to the `terraform.tfvars` file, filling in the required values for each:

```hcl
region = ""
project_name = ""
vpc_cidr = ""
pub_sub_1a_cidr = ""
pub_sub_2b_cidr = ""
pri_sub_3a_cidr = ""
pri_sub_4b_cidr = ""
pri_sub_5a_cidr = ""
pri_sub_6b_cidr = ""
db_username = ""
db_password = ""
certificate_domain_name = ""
additional_domain_name = ""
```

## ✈️ Ready to Deploy Your Application on the Cloud ⛅

Now, let’s navigate to the project directory:

```sh
cd root
```

👉 First, install the necessary Terraform dependencies:

```sh
terraform init
```

Run the following command to preview the changes that will be made:

```sh
terraform plan
```

✨ Finally, apply the changes to deploy your application:

```sh
terraform apply
```

When prompted, type `yes` to approve the deployment.

---

**Thanks for reading! 😅**

---

This version removes the source citation, while keeping the rest of the content intact. Let me know if you need any further adjustments!
