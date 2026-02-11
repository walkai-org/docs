# Configuration

After creating your repository from the template, configure the GitHub Actions variables and secrets in:
`Settings` -> `Secrets and variables` -> `Actions`.

!!! note
    Workflows authenticate to AWS by assuming the role referenced in the `AWS_DEPLOY_ROLE_ARN` secret, so make sure it matches the IAM role created during installation.

## Variables

- `AWS_S3_TERRAFORM_BUCKET`: name of the S3 bucket defined in the prerequisites for Terraform state.
- `BOOTSTRAP_FIRST_USER`: email address of the initial admin user.
- `VPC_CIDR`: address space for the VPC. Default is `172.31.0.0/16`.
- `K8S_CLUSTER_URL`: URL of the Kubernetes cluster.
- `BASE_DOMAIN`: your registered web domain.
- `EXTERNAL_DNS`: `true` if the domain is already managed externally, `false` if the root domain must be managed by this deployment.
- `SMTP_HOST`: hostname of your SMTP provider (for example, `smtp.azurecomm.net`).
- `SMTP_PORT`: SMTP port that supports STARTTLS (commonly `587`, but some providers use `25` or `2525`).
- `SMTP_USERNAME`: SMTP username (for example, `apikey` or `user@mycompany.com`).
- `MAIL_FROM`: sender shown in emails (recommended format: `Name <email@domain>`).

## Secrets

- `AWS_DEPLOY_ROLE_ARN`: ARN of the IAM role created during installation.
- `K8S_CLUSTER_TOKEN`: token used to access the Kubernetes cluster.
- `SMTP_PASSWORD`: password or token for `SMTP_USERNAME`.

