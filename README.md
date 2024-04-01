<!-- omit in toc -->
# Set Environment Action

<!-- omit in toc -->
## Content

- [Overview](#overview)
- [Environments](#environments)
- [Environment mapping](#environment-mapping)
- [Usage](#usage)
  - [Examples](#examples)

## Overview

This GitHub Action facilitates setting the environment name based on the
triggering workflow and git ref.

> ⚠️ This action is tailored for our specific needs and development workflow,
> at this moment, you cannot change the triggering events or the environment
> tag. Only use if you can adapt it on your workflow!

The action exposes the environment name value in two different contexts:

- Outputs
- [env context]

## Environments

This action can output the following environment names:

- `stage`
- `uat`
- `production`

## Environment mapping

The environment name mapping is as follows:

- Push event on a branch with name `main` -> Outputs environment **stage**.
- Tag push matching `v[0-9].[0-9].[0-9]-uat.[0-9]` -> Outputs **uat**.
- Tag matching `v[0-9].[0-9].[0-9]` -> Outputs **production**.

The Action uses `github.ref_name` to determine the branch or tag name.

## Usage

See [action.yml](action.yml) for more info about the action.

```yaml
- uses: gbh-tech/set-environment-action@v0.0.1
  id: env
```

### Examples

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: gbh-tech/set-environment-action@v0.1.1
        id: env
      # Using outputs
      - name: Show the selected environment name using output
        env:
          ENVIRONMENT: ${{ steps.env.outputs.environment }}
        run: echo "Environment is ${ENVIRONMENT}"
      # Using env. context
      - name: Show the selected environment name using env context
        run: echo "Environment is ${{ env.ENVIRONMENT }}"
```

<!-- References -->
[env context]: https://docs.github.com/en/actions/learn-github-actions/contexts#example-contents-of-the-env-context
