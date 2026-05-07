---
name: maintenance-check-aws-eks
description: Check everything that can be upgraded for maintenance across an AWS EKS Kubernetes cluster and output a decision file with Jira-formatted tickets for any required upgrades.
---

# Maintenance Check AWS EKS Skill

Run this skill when asked to check for available upgrades across an AWS EKS Kubernetes cluster for maintenance. It gathers current versions, compares them against latest available versions, and writes a decision file with Jira-formatted tickets for any required maintenance upgrades.

## Configuration

Update the table below with your cluster names, AWS profiles, and region before using this skill:

- **AWS region:** `<your-aws-region>` (e.g. `us-east-1`, `eu-west-1`)

| Environment | Cluster Name | AWS Profile |
|---|---|---|
| Staging / Non-Prod | `<your-staging-cluster>` | `<your-nonprod-profile>` |
| Production | `<your-prod-cluster>` | `<your-prod-profile>` |

If the user does not specify an environment, ask before proceeding.

---

## Step 1 — Check current EKS cluster version

```bash
aws eks describe-cluster \
  --name <cluster-name> \
  --profile <profile> \
  --region <your-aws-region> \
  --query "cluster.version" \
  --output text
```

Record: **current cluster version** (e.g. `1.29`).

---

## Step 2 — Determine latest available EKS version

```bash
aws eks describe-addon-versions \
  --profile <profile> \
  --region <your-aws-region> \
  --query "sort_by(addons[?addonName=='vpc-cni'].addonVersions[].compatibilities[].clusterVersion, &@) | [-1]" \
  --output text
```

Record: **latest available EKS version**.

**Decision point:** If `current version == latest version`, no maintenance upgrade is needed at all — skip Steps 3–4 and proceed to Step 5 with a "no maintenance upgrade required" output.

---

## Step 3 — Check EKS add-on versions

List installed add-ons and their current vs. latest versions compatible with the **target** cluster version:

```bash
# List installed add-ons
aws eks list-addons \
  --cluster-name <cluster-name> \
  --profile <profile> \
  --region <your-aws-region> \
  --output json

# Current version of each add-on
aws eks describe-addon \
  --cluster-name <cluster-name> \
  --addon-name <addon-name> \
  --profile <profile> \
  --region <your-aws-region> \
  --query "addon.addonVersion" \
  --output text

# Latest version compatible with the target cluster version
aws eks describe-addon-versions \
  --addon-name <addon-name> \
  --kubernetes-version <target-version> \
  --profile <profile> \
  --region <your-aws-region> \
  --query "addons[0].addonVersions[0].addonVersion" \
  --output text
```

Record for each add-on: **name**, **current version**, **latest compatible version**.

---

## Step 4 — Check EKS node group AMI versions

```bash
# List node groups
aws eks list-nodegroups \
  --cluster-name <cluster-name> \
  --profile <profile> \
  --region <your-aws-region> \
  --output json

# Current AMI release version per node group
aws eks describe-nodegroup \
  --cluster-name <cluster-name> \
  --nodegroup-name <nodegroup-name> \
  --profile <profile> \
  --region <your-aws-region> \
  --query "{releaseVersion: nodegroup.releaseVersion, version: nodegroup.version}" \
  --output json
```

To find the latest AMI for the target version, take the current AMI name, replace the EKS version with the target version followed by a `-*`:

```bash
aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=<ami-name-prefix>-<target-version>-*" \
  --profile <profile> \
  --region <your-aws-region> \
  --query "sort_by(Images, &CreationDate)[-1].{ImageId: ImageId, Name: Name}" \
  --output json
```

For example, if the current AMI name is `amazon-eks-node-al2023-x86_64-standard-1.34-v20260107`, the filter value becomes `amazon-eks-node-al2023-x86_64-standard-1.35-*`.

Record for each node group: **name**, **current AMI release version**, **latest AMI release version**.

---

## Step 5 — Write the output file

Write all findings to a Markdown file at:

```
./maintenance-checks/maintenance-check-aws-eks-<environment>-<YYYY-MM-DD>.md
```

in the current working directory (the project root).

### Output file format

```markdown
# Maintenance Upgrade Check — <environment> — <YYYY-MM-DD>

## Decision

**Maintenance upgrade required: Yes / No**

<If no maintenance upgrade required>
The EKS cluster is already on the latest Kubernetes version. No maintenance upgrade is needed.
</If no maintenance upgrade required>

---

## Summary

| Component | Current | Latest |
|---|---|---|
| EKS Cluster (<cluster-name>) | <current> | <latest> |
| EKS Add-on: <addon-name> | <current> | <latest compatible with target> |
| Node Group: <nodegroup-name> AMI | <current> | <latest> |

---

<If maintenance upgrade required>

---

## Jira Ticket

**Title:** Upgrade EKS cluster <cluster-name> from <current> to <target>

**Parent:** `<your-jira-parent-ticket>`

**Label:** `<your-jira-label>`

**Story Points:** `<your-story-points>`

**Refined:** Yes

**Description:**
The EKS cluster `<cluster-name>` is currently running Kubernetes `<current>`.
AWS has released `<target>` as the next supported minor version.

The following work is required:

**1. EKS Control Plane**
- Upgrade EKS control plane from `<current>` to `<target>`
- Verify cluster health post-upgrade

**2. EKS Add-ons**

| Add-on | Current Version | Target Version |
|---|---|---|
| <addon-name> | <current> | <latest compatible with target> |

- Update each add-on above to its latest version compatible with `<target>`
- Verify add-on health post-update

**3. Node Group AMIs**

| Node Group | Current AMI Release | Latest AMI Release |
|---|---|---|
| <nodegroup-name> | <current> | <latest> |

- Update each node group to use the latest AMI release for EKS `<target>`
- Perform rolling node replacement and verify workload health

**References:**
- AWS EKS version lifecycle: https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html

</If maintenance upgrade required>
```

---

## Rules

- Always use the configured `--region` and the correct `--profile` for the target environment (see table above). Never omit them.
- Never run any mutating AWS commands (`update-cluster`, `update-addon`, `update-nodegroup`, etc.) — this skill is read-only. A human must execute the actual maintenance upgrades.
- If an AWS command fails (e.g. access denied, unknown region), stop and report the error to the user before continuing.
- If the cluster is already on the latest Kubernetes version, skip Steps 3–4 entirely — add-ons and node group AMIs are only checked when an EKS cluster version upgrade is needed.
- Output the findings file even if no maintenance upgrade is required — the "No maintenance upgrade is needed" record is itself useful evidence for planning.
- Do not include AWS account IDs, ARNs, or any credential-like strings in the output file.
- Only one Jira ticket is ever produced, covering the control plane, add-ons, and node group AMIs together.
