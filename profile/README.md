[![Jaws: one platform for every project on AWS](https://raw.githubusercontent.com/Jaws-Platform/jaws-branding/main/assets/banner/jaws-readme-banner.png)](https://github.com/Jaws-Platform/jaws)

**Jaws creates and manages the AWS infrastructure for every project**: static
sites, Lichess bots, research workloads and Databricks workspaces. Each one
gets the same governance, tagging, cost allocation, state handling and
least-privilege CI roles, without building them again.

### How a project uses Jaws

1. **Onboard it** in [`jaws`](https://github.com/Jaws-Platform/jaws). The
   project is registered, which creates its CI roles, its prefix in the shared
   Terraform state bucket and its guardrails.
2. **Declare what it needs** in one `jaws.yaml` at the root of its repository.
   The `jaws` CLI derives everything else: region, account, networking, names,
   state and roles.
3. **Run it from CI** with the Jaws reusable workflows, pinned to a Jaws
   release. The project's repository keeps no Terraform.

### Repositories

| Repository | What it is | Registry |
|---|---|---|
| [`jaws`](https://github.com/Jaws-Platform/jaws) | The platform: the `jaws` CLI, its reusable workflows, the project registry and the foundation stack | |
| [`terraform-aws-jaws-foundation`](https://github.com/Jaws-Platform/terraform-aws-jaws-foundation) | An AWS account's foundation: naming and tagging, the state bucket, private buckets, service-linked roles and IAM Identity Center access | [`jaws-foundation`](https://registry.terraform.io/modules/Jaws-Platform/jaws-foundation/aws/latest) |
| [`terraform-aws-jaws-network`](https://github.com/Jaws-Platform/terraform-aws-jaws-network) | A small, low-cost network: a VPC with public and optional private subnets, and a NAT instance | [`jaws-network`](https://registry.terraform.io/modules/Jaws-Platform/jaws-network/aws/latest) |
| [`terraform-aws-jaws-ci`](https://github.com/Jaws-Platform/terraform-aws-jaws-ci) | GitHub Actions on AWS without long-lived keys: OIDC roles for each kind of job, and a catalog of least-privilege policies | [`jaws-ci`](https://registry.terraform.io/modules/Jaws-Platform/jaws-ci/aws/latest) |
| [`terraform-aws-jaws-static-site`](https://github.com/Jaws-Platform/terraform-aws-jaws-static-site) | Static sites on S3 and CloudFront, with production, dev and per-pull-request preview environments | [`jaws-static-site`](https://registry.terraform.io/modules/Jaws-Platform/jaws-static-site/aws/latest) |
| [`terraform-aws-jaws-research`](https://github.com/Jaws-Platform/terraform-aws-jaws-research) | Research workloads: ephemeral EC2 task runners for CI jobs, and append-only S3 dataset buckets | [`jaws-research`](https://registry.terraform.io/modules/Jaws-Platform/jaws-research/aws/latest) |
| [`terraform-aws-jaws-lichess-bot`](https://github.com/Jaws-Platform/terraform-aws-jaws-lichess-bot) | A Lichess bot on one EC2 instance, with graceful deploys and game records shipped to S3 | [`jaws-lichess-bot`](https://registry.terraform.io/modules/Jaws-Platform/jaws-lichess-bot/aws/latest) |
| [`terraform-aws-jaws-databricks`](https://github.com/Jaws-Platform/terraform-aws-jaws-databricks) | Databricks on AWS with Unity Catalog: the account baseline, workspaces in their own VPC, isolated catalogs and governed compute | [`jaws-databricks`](https://registry.terraform.io/modules/Jaws-Platform/jaws-databricks/aws/latest) |
| [`terraform-aws-jaws-monitoring`](https://github.com/Jaws-Platform/terraform-aws-jaws-monitoring) | Self-hosted monitoring and multi-source cost reporting, normalized to the FinOps FOCUS schema | not yet published |
| [`jaws-branding`](https://github.com/Jaws-Platform/jaws-branding) | The logo, the avatars, the README banner and the README header standard | |

The module families are plain Terraform with no dependency on Jaws:
everything is an input, so they work in any stack.

### Bots

| GitHub App | What it does |
|---|---|
| [`jaws-platform-release-bot`](https://github.com/apps/jaws-platform-release-bot) | Opens the Release Please pull requests, so CI runs on them |
| [`jaws-platform-comments`](https://github.com/apps/jaws-platform-comments) | Posts the Jaws plan and preview comments on pull requests |

### How every repository works

One-line Conventional Commits, every change through a pull request, squash or
rebase merges only, and a release cut by merging the Release Please pull
request. Everything is MIT licensed.
