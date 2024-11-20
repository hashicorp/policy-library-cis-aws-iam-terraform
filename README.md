# Sentinel Policies for the CIS AWS Foundations Benchmark

At HashiCorp, we’re committed to making policy management easier for our customers. We understand that developing policies from scratch can be time-consuming and resource-intensive. To address this, we’re introducing our Prewritten Policy Libraries—expertly crafted, ready-to-use policies designed to streamline your compliance processes and enhance security across your infrastructure.

This repository contains several policy libraries designed to accelerate the adoption of the CIS AWS Foundations Benchmark within HCP Terraform. These libraries can be utilized to enforce best practices and security standards across your AWS environment.

For more details on how to work with these policies and to understand the Sentinel language and framework, please refer to the [Sentinel documentation](https://developer.hashicorp.com/sentinel/) or the README documentation included with each of the policy [libraries](./libraries/).

## Table of Contents

- [Getting Started](#getting-started)
- [Documentation](#documentation)
- [Feedback](#feedback)
- [Resources](#resources)

## Getting Started

This getting started guide assumes that:

1. You are familiar with core workflows in HCP Terraform and Terraform Enterprise, and you have an existing workspace configured with AWS access credentials.

   > **Tip:** If you do not have these prerequisites, please refer to the [Use VCS-Driven Workflow](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-vcs-change) and [Create a Variable Set](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-create-variable-set) tutorials for guidance.

2. You have a user account that is part of the ["owners"](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/permissions#organization-owners) team or have ["Manage Policies"](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/permissions#manage-policies) organization-level permissions to create new policy sets and policies.

3. Ensure you are using HCP Terraform or Terraform Enterprise [v202312-1](https://developer.hashicorp.com/terraform/enterprise/releases/2023/v202312-1) or a later version.


For your convenience, we have included a [policy-set](./modules/policy-set/) Terraform module that utilizes the Terraform Enterprise provider to create a new [Policy Set Version](https://developer.hashicorp.com/terraform/cloud-docs/api-docs/policy-sets#create-a-policy-set-version) for each Sentinel-based policy library. When applied, the provider will automatically package the contents of each library into a tar file and upload it to HCP Terraform.

You can see how to use this module by reviewing the [get-started](./get-started/) code provided:

**Example:**
```terraform
locals {
  policy_libraries = toset([for f in fileset(path.cwd, "../**/sentinel.hcl") : dirname(f)])
}

module "policy-set" {
  source   = "../modules/policy-set"
  for_each = local.policy_libraries

  policy_set_name            = upper(split("policy-library-", each.value)[1])
  organization_name          = var.tfe_organization
  policy_set_workspace_names = ["foo", "bar"]
  policy_library_source_path = each.value
}
```

By default, the module will enable all policies within the library, and they will be enforced by the HCP Platform with the `enforcement_level` set to `advisory` only.

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

If you want to enable only a subset of the policies or change the [enforcement levels](https://developer.hashicorp.com/sentinel/docs/concepts/enforcement-levels) to either `soft-mandatory` or `hard-mandatory`, we recommend updating the contents of the `sentinel.hcl` file in each library before applying the Terraform configuration.

> **Important:**
The policies in each library are opinionated and depend on several Sentinel modules. To learn more about modules, please refer to the [Sentinel module](https://developer.hashicorp.com/sentinel/docs/extending/modules) documentation.
>
To learn more about how to configure a policy set as a [policy evaluation](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement/manage-policy-sets#policy-evaluations), please review the Terraform Enterprise provider [documentation](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/policy_set#agent_enabled).

## Documentation

The documentation for the policy libraries consists of Markdown files within this GitHub repository. The Markdown documents are organized in the following locations within each [library](./libraries/):

| Location       | Filename           | Description                            |
|----------------|--------------------|----------------------------------------|
| /              | README.md          | Index page for the policy library.     |
| docs/policies/ | <policy name\>.md | Description of a single policy.        |
| docs/modules/  | <module name\>.md | Description of a single policy module. |

> **Note:** The <policy_name> and <module_name> in the Markdown filenames correspond to the names specified for each policy and module block in the library’s sentinel.hcl file.


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

### Policies supported

The following CIS standards have equivalent Sentinel policies written for them. These policies get enforced between the `plan` and `apply` phase whenever a run gets queued in TFC/TFE.

- iam-no-admin-privileges-allowed-by-policies
- iam-no-policies-attached-to-users
- iam-password-expiry
- iam-password-length
- iam-password-numbers
- iam-password-lowercase
- iam-password-reuse
- iam-password-symbols
- iam-password-uppercase

### Minimum Sentinel Version Required

```pre
0.26.x
```