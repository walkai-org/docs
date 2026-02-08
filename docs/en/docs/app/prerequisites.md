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
6. An IAM OpenID Connect identity provider for GitHub Actions (`token.actions.githubusercontent.com`) in that AWS account.

The identity provider is created once per AWS account. Follow the official GitHub guide for AWS OIDC setup: [Configuring OpenID Connect in AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws).

During installation, you will use this provider to create the deployment role and reference the S3 bucket name in its policy.
