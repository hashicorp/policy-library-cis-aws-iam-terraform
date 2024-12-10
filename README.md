# (Beta) Pre-written Sentinel Policies for Center for Internet Security(CIS) AWS IAM Foundations Benchmark

Pre-written Sentinel policies are ready to use compliance checks for CIS AWS Foundations Benchmarking v1.2, v1.4 and v3.0 to help enable your AWS resources meet industry security standards.

At HashiCorp, we’re committed to making policy management easier for our customers. We understand that developing policies from scratch can be time-consuming and resource-intensive. To address this, we’re introducing our Prewritten Policy Libraries—expertly crafted, ready-to-use policies designed to streamline your compliance processes and enhance security across your infrastructure.

This repository contains several policies designed to accelerate the adoption of the CIS AWS Foundations Benchmark within HCP Terraform. These policies can be utilized to enforce best practices and security standards across your AWS environment.

For more details on how to work with these policies and to understand the Sentinel language and framework, please refer to the [Sentinel documentation](https://developer.hashicorp.com/sentinel/) or the README documentation included with each of the policy [libraries](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies).

## Feedback

We aim to validate the effectiveness of our policies by collecting diverse user feedback and understanding real-world use cases. This input will help refine our policies and enhance their overall impact. 

1. You can submit your feedback via a [public beta survey](https://docs.google.com/forms/d/e/1FAIpQLScswwLMaVaRuYRGJzDjNiycwM4BUa_gAIsAE_zOPdgyFeLXCA/viewform).

2. If you have any issues or enhancement suggestions to the library, please create [a new GitHub issue](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/issues/new).

3. Alternatively, we welcome any contributions that improve the impact of this library! To learn more about contributing and suggesting changes to this library, refer to the [contributing guide](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/CONTRIBUTING.md).

### Policies Included

- IAM policies should not allow full "*" administrative privileges ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-no-admin-privileges-allowed-by-policies.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-no-admin-privileges-allowed-by-policies.sentinel))
- IAM users should not have IAM policies attached ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-no-policies-attached-to-users.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-no-policies-attached-to-users.sentinel))
- AWS IAM Password Policy should expire passwords within 90 days or less ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-expiry.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-expiry.sentinel))
- AWS IAM Password Policy requires minimum password length of 14 or greater ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-length.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-length.sentinel))
- AWS IAM Password Policy requires at least one lowercase letter ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-lowercase.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-lowercase.sentinel))
- AWS IAM Password Policy requires at least one number ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-numbers.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-numbers.sentinel))
- AWS IAM Password Policy prevents password reuse ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-reuse.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-reuse.sentinel))
- AWS IAM Password Policy requires at least one symbol ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-symbols.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-symbols.sentinel))
- AWS IAM Password Policy requires at least one uppercase letter ([docs](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/docs/policies/iam-password-uppercase.md) | [code](https://github.com/hashicorp/policy-library-cis-aws-iam-terraform/blob/main/policies/iam-password-uppercase.sentinel))

## Getting Started

This getting started guide assumes that:

1. You are familiar with core workflows in HCP Terraform and Terraform Enterprise, and you have an existing workspace configured with AWS access credentials.

2. You have a user account that is part of the ["owners"](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/permissions#organization-owners) team or have ["Manage Policies"](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/permissions#manage-policies) organization-level permissions to create new policy sets and policies.

3. You are using HCP Terraform or Terraform Enterprise [v202406-1](https://developer.hashicorp.com/terraform/enterprise/releases/2024/v202406-1) or a later version.

4. You are using Sentinel version 0.26.x and later version.

By default, all the policies within the library will be enforced by the HCP Platform with the `enforcement_level` set to `advisory` only.

**Example:**
```
policy "iam-password-expiry" {
  source = "./policies/iam-password-expiry.sentinel"
  enforcement_level = "advisory"
   params = {
      password_expiry_days = 90
   }
}
```

If you want to change the [enforcement levels](https://developer.hashicorp.com/sentinel/docs/concepts/enforcement-levels) to either `soft-mandatory` or `hard-mandatory`, we recommend updating the contents of the `sentinel.hcl` file in each library before applying the Terraform configuration.

> **Important:**
The policies in each library are opinionated and depend on a [Sentinel module](https://developer.hashicorp.com/sentinel/docs/extending/modules) documentation.
>
To learn more about how to configure a policy set as a [policy evaluation](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement/manage-policy-sets#policy-evaluations), please review the Terraform Enterprise provider [documentation](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/policy_set#agent_enabled).

## Resources

- [Get Started - HCP Terraform](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started)
- [Connecting VCS Providers to HCP Terraform](https://developer.hashicorp.com/terraform/cloud-docs/vcs)
- [Policy Enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [Managing Policy Sets](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement/manage-policy-sets)
- [Introduction to Sentinel](https://developer.hashicorp.com/sentinel/intro/what)
- [Sentinel Documentation](https://developer.hashicorp.com/sentinel/docs)
- [Sentinel Language](https://developer.hashicorp.com/sentinel/docs/language/)
- [Sentinel Language Specification](https://developer.hashicorp.com/sentinel/docs/language/spec)
- [Policy Libraries](https://registry.terraform.io/browse/policies)
