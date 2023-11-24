# Crossplane Dynamic Diff GitHub Action

This GitHub Action performs a dynamic diff between pull requests (PRs) and the target branch for Crossplane YAML files with specific annotations. It leverages the Crossplane CLI to render compositions and functions specified in the annotations. Differences are then compared between the PR and target branches, and a detailed diff is generated and commented on the PR.

## Usage

To use this GitHub Action, add the following step to your GitHub job:

```yaml
name: Example Workflow
on:
  pull_request:

jobs:
  example:
  runs-on: ubuntu-latest
  steps:
    - name: Crossplane Dynamic Diff
      uses: upbound/crossplane-diff-action@main
      with:
        # process XRs from the given directory
        dir: examples 
        github-token: ${{ secrets.GITHUB_TOKEN }}
  
```


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

Special thanks to the Crossplane community for the development and maintenance of the Crossplane CLI.
