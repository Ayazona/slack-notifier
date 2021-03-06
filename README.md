# Slack notifier

This project provides a simple command line tool that reads status check updates
from the Github API and posts or updates a Slack message.

## Usage

This script is intended to be run as part of a Github Workflow. Here's an example
workflow:

```yaml
name: Slack status updater

on: [push]

jobs:
  post_slack_status:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Python
        uses: actions/setup-python@v1
        with:
          python-version: 3.8
      - name: Download executable
        run: curl -sSL -o ./notifier.pyz "https://github.com/kolonialno/slack-notifier/releases/download/v1.0.1/notifier.pyz"
      - name: Run notifier
        run: python ./notifier.pyz --repo ${{ github.repository }} --channel "#my-slack-channel" --commit ${{ github.sha }}
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SLACK_TOKEN: ${{ secrets.SLACK_TOKEN }}
```

## Creating a new release

 1. Update the example above to use the new version and commit
 2. Create a new tag, e.g. `git tag v1.2.3`
 3. Push commit with tags, the workflows will do the rest: `git push origin --tags`
