# commit-message-format

## installing an actions
1. Enable github actions on your repo
In the left sidebar, click Actions, then click General. Under "Workflow permissions", use the Allow GitHub Actions to create and approve pull requests setting to configure whether GITHUB_TOKEN can create and approve pull requests.


2. First create a directory
`mkdir -p .github/workflows



3. We are using https://github.com/marketplace/actions/conventional-commit
Create a file conventional_commit.yaml

`name: Pull request

on:
  pull_request: {}

jobs:
  Validate:
    runs-on: ubuntu-latest
    steps:
      - name: Validate PR title
        uses: lab42/conventional-commit@main
`