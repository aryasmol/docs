---
title: "Overview"
description: "Manage your Atoms agents from the command line."
---

The Atoms CLI is the primary way to manage, test, and deploy your agents. It allows you to initialize projects, run local servers, and push your code to the Atoms platform.

## Installation

The CLI is included with the main SDK package.

```bash
pip install smallestai
```

## Verification

To verify your installation, run:

```bash
python -m smallestai.cli.main --help
```

You should see an output similar to this:

```text Terminal Output
Usage: python -m smallestai.cli.main [OPTIONS] COMMAND [ARGS]...

  SmallestAI CLI

╭─ Options ────────────────────────────────────────────────────────────╮
│ --install-completion          Install completion for the current     │
│                               shell.                                 │
│ --show-completion             Show completion for the current shell, │
│                               to copy it or customize the            │
│                               installation.                          │
│ --help                        Show this message and exit.            │
╰──────────────────────────────────────────────────────────────────────╯
╭─ Commands ───────────────────────────────────────────────────────────╮
│ agent                                                                │
│ auth                                                                 │
╰──────────────────────────────────────────────────────────────────────╯
```

## Basic Workflow

<Steps>
  <Step title="Login">
    Authenticate with your Smallest AI account.
    `smallestai auth login`
  </Step>
  <Step title="Initialize">
    Link your local directory to a created agent.
    `smallestai agent init`
  </Step>
  <Step title="Deploy">
    Push your code to the cloud.
    `smallestai agent deploy`
  </Step>
</Steps>
