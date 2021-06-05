---
title: "pre-commit hooks"
draft: false
type: docs
layout: single

menu:
  docs:
    weight: 120
---

# pre-commit hooks

`pre-commit` is a tool which manages running of
[git hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) scripts.
See the official documentation for more information: [pre-commit.com](https://pre-commit.com/)

Below you can see the available `pre-commit` hooks you can use.


{{% note %}}
If you specify the `args:` section for a hook in your pre-commit config
the default `args:` are overwritten. So if you want to add arguments
you need to specify the default ones in your config also.
{{% /note %}}


## poetry-check

The `poetry-check` hook calls the `poetry check` command
to make sure the poetry configuration does not get committed in a broken state.

### Arguments

The hook takes the same arguments as the poetry command.
For more information see the [check command](/docs/cli#check).


## poetry-lock

The `poetry-lock` hook calls the `poetry lock` command
to make sure the lock file is up-to-date when committing changes.

### Arguments

The hook takes the same arguments as the poetry command.
For more information see the [lock command](/docs/cli#lock).


## poetry-export

The `poetry-export` hook calls the `poetry export` command
to sync your `requirements.txt` file with your current dependencies.

{{% note %}}
It is recommended to run the [`poetry-lock`](#poetry-lock) hook prior to this one.
{{% /note %}}

### Arguments

The hook takes the same arguments as the poetry command.
For more information see the [export command](/docs/cli#export).

`args: ["-f", "requirements.txt", "-o", "requirements.txt"]` are the default arguments
which will create/update the requirements.txt file in the current working directory.

For output to the console change the arguments and add `verbose: true` to `poetry-export`
in your `.pre-commit-config.yaml` file like so:

```yaml
hooks:
  - id: poetry-export
    args: ["-f", "requirements.txt"]
    verbose: true
```

Or to put the `dev` dependencies into the `requirements.txt` also use this:

```yaml
hooks:
  - id: poetry-export
    args: ["--dev", "-f", "requirements.txt", "-o", "requirements.txt"]
```


## Usage

For how to use `pre-commit` please see the [official documentation](https://pre-commit.com/).

Example `.pre-commit-config.yaml` file:

```yaml
repos:
  - repo: https://github.com/python-poetry/poetry
    rev: ''  # add version here
    hooks:
      - id: poetry-check
      - id: poetry-lock
      - id: poetry-export
        args: ["-f", "requirements.txt", "-o", "requirements.txt"]
```