# Terraform Notes

* **Always run `terraform plan` before `terraform apply`.** Review anything marked for destruction or replacement before approving changes.

* **Follow least privilege.** DONT use broad IAM permissions such as `"Action": "*"` and `"Resource": "*"`. Grant only the actions and resources each workload needs.

* **Never git commit secrets or state files.** Keep `terraform.tfstate`, `.terraform/`, secret `.tfvars` files, credentials, and private keys out of github.  **git.ignore**

* **Use variables instead of hardcoding values.** Store regions, environments, project names, and reusable settings in `variables.tf` or environment-specific `.tfvars` files.

* **Use consistent resource references.** `var.example`, `local.example`, `aws_resource.example.id`, `data.aws_resource.example.id`, and `module.example.output`.

* **Keep Terraform organized.** `versions.tf`, `provider.tf`, `variables.tf`, `main.tf`, `iam.tf`, `outputs.tf`, and `locals.tf`.

* **Format and validate before deploying.**

```bash
terraform fmt -recursive
terraform validate
terraform plan
```

* **Use remote state for shared projects.** Store state securely in a remote backend, enable encryption and versioning, and keep dev, staging, and production state separate. (security Best practice)



