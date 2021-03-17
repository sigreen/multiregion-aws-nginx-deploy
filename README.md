# Multiregion AWS Nginx Provisioning with Terraform

This project demonstrates a simple way to provision Nginx across multiple AWS regions / availability zones.  For this example, we use Terraform workspaces to switch between region / AZ deployments.

## Procedure

In this example, we start off by deploying to `us-east-2`.

```
terraform workspace new us-east-2 # Create a unique directory for us-east-2
terraform init # Initialize a Terraform working directory
terraform plan # Generate and show an execution plan
terraform apply # Builds or changes infrastructure
```

Test that nginx is running by navigating to the public IP via the web browser e.g. `http://<publicIP>:80`

To test the multiregion support, we can now try deploying to `us-west-2`:
```
terraform workspace new us-west-2 # Create a unique directory for us-west-2
terraform init -var "region=us-west-2" -var "availability_zone=us-west-2a" -var "instance_ami=ami-6cd6f714" -var "public_key_name=simonPublicKey"  # Initialize a Terraform working directory
terraform plan -var "region=us-west-2" -var "availability_zone=us-west-2a" -var "instance_ami=ami-6cd6f714" -var "public_key_name=simonPublicKey" # Generate and show an execution plan
terraform apply -var "region=us-west-2" -var "availability_zone=us-west-2a" -var "instance_ami=ami-6cd6f714" -var "public_key_name=simonPublicKey" # Builds or changes infrastructure
```

Test that nginx is running by navigating to the public IP via the web browser e.g. `http://<publicIP>:80`

That's it!  Once we're done with testing, don't forget to tear everything down:

```
terraform destroy -var "region=us-west-2" -var "availability_zone=us-west-2a" -var "instance_ami=ami-6cd6f714" -var "public_key_name=simonPublicKey"  : Destroy Terraform-managed infrastructure
terraform workspace select us-east-2
terraform destroy
```
