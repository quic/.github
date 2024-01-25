# QuIC GitHub Organization

This repo contains workflow templates and other org level config and files

## QuIC Repolinter GitHub Action

QuIC repos should enable the Repolinter GitHub Action upon creation. This action runs the Repolinter tool, which lints open source repositories for common issues.

### Initial setup

1. Navigate to the new repo
1. Click on Actions
1. If you have existing actions in the repo, click "New workflow", else skip to next step
1. Locate the "By Qualcomm Innovation Center" section and click "Configure" under "QuIC Organization Repolinter Workflow"
1. Click "Commit changes...", select "Commit directly to the main branch" (or feel free to create a new branch and start a PR), ensure your QuIC email is selected under "Commit Email", and then click "Sign off and commit changes"
1. This will create a GitHub Action config file in your repo under the path `.github/workflows/quic-organization-repolinter.yml`
1. Adjust it as needed, e.g. the Repolinter action is configured to run on Push and Pull Requests into the main/master branch, but you may want to further adjust when it runs.

### Customizing Repolinter rules

When the GitHub Action is run, it first checks your QuIC repo for a local `repolint.json` file at the root directory. If it doesn't find one it'll use the default QuIC Repolinter ruleset, which is located here https://github.com/quic/.github/blob/main/repolint.json

To customize the default QuIC repolinter ruleset (e.g. to add some language specific file extensions for the license check), copy the `repolint.json` file (from https://github.com/quic/.github/blob/main/repolint.json) and place it at the root of your QuIC repo. Now you can edit and tweak it as applicable.
