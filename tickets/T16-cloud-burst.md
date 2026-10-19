# T16 — Cloud burst reprocessing
Status: todo · Arc 4

## Before anything else
AWS Budgets alerts at $10 / $30 / $60 / $90. Set a **cost target** for the campaign and write it
down before starting.

## Setup
Terraform written by hand, with no community modules the first time:
- A VPC with public subnets and **no NAT Gateway**
- EKS
- IAM so that pods can read from S3 (EKS Pod Identity or IRSA, and know why you picked one)
- Karpenter with a spot NodePool
- Kueue

Put the dataset in S3 (uploading is free; downloading is not).

## Done when
- The reprocessing campaign completes **under your cost target**. Estimate it beforehand with
  Infracost and eks-node-viewer, then check Cost Explorer the next day.
- It survives spot interruptions injected with AWS FIS: jobs checkpoint and resume, and the
  ledger shows no double-counted output.
- Only the results (histograms) come back home, never the raw data.
- After `terraform destroy`, a Steampipe query or an aws-nuke dry-run shows **zero** leftover
  resources.

## Read after
- Karpenter: consolidation and disruption
- EKS Pod Identity vs IRSA
- How spot interruption notices work
- The AWS IAM policy evaluation logic documentation
