# Installation

The application infrastructure is deployed from a Terraform-based GitHub template repository.

1. Open the [walkai-infra](https://github.com/walkai-org/walkai-infra) template repository.
2. Click **Use this template** to create your own repository based on it, and name it as you prefer.
3. In AWS IAM, create a new role for GitHub Actions and set the trust relationship policy using [this JSON](../walkai-infra-trust-policy.json).
4. Before creating the trust policy, replace `<your-aws-account-id>` and `<org>/<repo>` with your AWS account ID and the GitHub repository you created in step 2.
5. Create a custom policy from [this JSON](../walkai-infra-policy.json).
6. Before creating the policy, replace `<your-terraform-state-bucket>` in the first two statements with the S3 bucket name from the prerequisites.
7. Attach the custom policy to the role. The policy follows the principle of least privilege, granting only the permissions needed by the deployment pipeline.
8. Save the role ARN. You will set it as the `AWS_DEPLOY_ROLE_ARN` secret in the configuration.

You will use this repository in the next sections to configure GitHub Actions and run the deployment workflows.
