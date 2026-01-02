---
title: "CLI Reference"
description: "Deploy your agents to the SmallestAI platform."
---

The CLI is used to deploy your agent to SmallestAI infrastructure. If you just want to run locally, you don't need the CLI—just run `python main.py`.

<Note>
  **When to use the CLI**: Use the CLI when you want SmallestAI to host your agent in the cloud for production use, making it accessible via API or phone calls.
</Note>

## Installation

The CLI is included with the SDK.

```bash
pip install smallestai
```

Verify with:
```bash
smallestai --help
```

---

## Auth

### `login`

Authenticate with your SmallestAI account.

```bash
smallestai auth login
```

### `logout`

Clear your local credentials.

```bash
smallestai auth logout
```

---

## Agent Commands

### `init`

Link your local directory to an agent on the platform.

<Note>
  **Prerequisite:** You must first create an agent on the [Atoms Console](https://atoms.smallest.ai). This command will prompt you to select from your existing agents.
</Note>

```bash
smallestai agent init
```

### `deploy`

Package and deploy your agent code to the cloud.

```bash
smallestai agent deploy --entry-point main.py
```

**Options:**
- `-e, --entry-point`: The file to run. Defaults to `server.py`, but you can specify any file.

### `chat`

Start an interactive chat session with your **locally running** agent (connects to `localhost:8080`).

```bash
smallestai agent chat
```

### `builds`

View deployment history and manage which version is serving traffic.

```bash
smallestai agent builds
```

---

## Going Live

Deployments are **not live** by default. To serve traffic:

1. Run `smallestai agent builds`
2. Select the build you want to promote
3. Choose **Make Live**

<Warning>
  **One Agent, One Live Build.** Making a new build live automatically replaces the previous one.
</Warning>

## Taking Down

To stop serving traffic:

1. Run `smallestai agent builds`
2. Select the **LIVE** build
3. Choose **Take Down**
