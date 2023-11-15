# Crossplane Dynamic Diff GitHub Action

This GitHub Action performs a dynamic diff between pull requests (PRs) and the main branch for Crossplane YAML files with specific annotations. It leverages the Crossplane CLI to render compositions and functions specified in the annotations. Differences are then compared between the PR and main branches, and a detailed diff is generated and commented on the PR.

## Usage

To use this GitHub Action, add the following YAML configuration to your repository's `.github/workflows` directory:

```yaml
name: Crossplane Dynamic Diff

on:
  pull_request:
    branches: [main]

jobs:
  pr:
    # ... (Same as in the provided workflow)

  main:
    # ... (Same as in the provided workflow)
```

Make sure to customize the configuration as needed for your repository.

## Workflow Overview

The workflow consists of two main jobs:

1. **pr (Pull Request) Job:**
   - Installs dependencies, including QEMU, Go, yq, and the Crossplane CLI.
   - Discovers YAML files in the `examples` directory and extracts composition and function paths from annotations (`crossplane.io/render-composition-path` and `crossplane.io/render-function-path`).
   - Executes a dynamic job for each input file, rendering compositions and functions and storing the output as `diff-pr-*.yaml`.
   - Sorts the YAML files and stores the sorted versions as artifacts.

2. **main Job:**
   - Installs dependencies, including QEMU, Go, yq, and the Crossplane CLI.
   - Checks out the main branch and downloads artifacts from the PR job.
   - Discovers YAML files in the `examples` directory and extracts composition and function paths from annotations (`crossplane.io/render-composition-path` and `crossplane.io/render-function-path`).
   - Executes a dynamic job for each input file, rendering compositions and functions and storing the output as `diff-main-*.yaml`.
   - Sorts the YAML files and stores the sorted versions as artifacts.
   - Compares the outputs between the PR and main branches, generating a diff for each file.
   - Generates a comment summarizing all differences and attaches the comment to the PR.

### Annotations

To utilize this GitHub Action, ensure that your Crossplane YAML files in the `examples` directory contain the following annotations:

- `crossplane.io/render-composition-path`: Specifies the path to the composition definition.
- `crossplane.io/render-function-path`: Specifies the path to the function definition.

These annotations provide the necessary information for the Crossplane CLI to render compositions and functions during the dynamic diff process.

### Example Comment Output

The generated comment on the PR includes a detailed diff for each changed file:

```diff
@@ -405,7 +405,7 @@
   generateName: ref-aws-network-
   labels:
     crossplane.io/composite: ref-aws-network
-    networks.aws.platform.upbound.io/network-id: platform-ref-aws
+    networks.aws.platform.upbound.io/network-id: platform-ref-aws-test
   ownerReferences:
   - apiVersion: aws.platform.upbound.io/v1alpha1
     blockOwnerDeletion: true
@@ -418,7 +418,7 @@
     cidrBlock: 192.168.0.0/16
     enableDnsHostnames: true
     enableDnsSupport: true
-    region: us-east-1
+    region: eu-central-1
     tags:
       Name: ref-aws-network
```

This information helps reviewers understand the changes introduced by the PR.

## Credits

This GitHub Action uses the [machine-learning-apps/pr-comment](https://github.com/machine-learning-apps/pr-comment) action for commenting on the PR. Special thanks to the Crossplane community for the development and maintenance of the Crossplane CLI.
