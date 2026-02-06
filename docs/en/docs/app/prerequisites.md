---
hide:

- toc

---

# Prerequisites

Before running the automatic deployment pipeline, make sure you have:

1. An SMTP email service with STARTTLS and username/password authentication.
2. A registered web domain (for example, `mycompany.com`).
3. A GitHub account.
4. An AWS account.
5. An S3 bucket in that AWS account to store the Terraform state.
6. An IAM role in that AWS account with the required [custom policy](../walkai-infra-policy.json) attached (open the JSON file and attach it in IAM).

**Note:** before attaching the policy, replace `<your-terraform-state-bucket>` in the first two statements with the S3 bucket name from item 5.

The policy follows the principle of least privilege, granting only the permissions needed by the deployment pipeline.
