---
title: "Commands"
description: "Reference guide for the smallestai CLI."
---

The `smallestai` command line tool helps you manage the entire lifecycle of your agents.

<Note>
  Ensure you have installed the package: `pip install smallestai`
</Note>

## Auth

Manage your credentials.

### `login`

Logs you in to the Atoms platform. You will be prompted to enter your API key.

```bash
smallestai auth login
```

### `logout`

Clears your local credentials.

```bash
smallestai auth logout
```

## Agent Lifecycle

### `init`

Initializes the current directory as an Atoms agent project.

```bash
smallestai agent init
```

### `deploy`

Packages and deploys your agent code to the Atoms infrastructure.

```bash
smallestai agent deploy [OPTIONS]
```

**Options:**
- `-e, --entry-point`: The file to run (default: `server.py`).

### `chat`

Starts an interactive chat session with your locally running agent (defaults to localhost:8080).

```bash
smallestai agent chat
```

## Managing Live Builds

Use the `builds` command to see deployment history and control which version of your agent is serving traffic.

```bash
smallestai agent builds
```

### Going Live (`Make Live`)

Deployments are **not live** by default. To serve traffic, you must explicitly promote a build.

1.  Run `smallestai agent builds`.
2.  Select the build you want to promote.
3.  Choose **Make Live**.

<Warning>
  **One Agent, One Live Build.**
  Only one build can be live at a time for a given agent. Making a new build live automatically replaces the previous one.
</Warning>

![SDK Live Indicator](/images/sdk-live.png)

### Taking Down (`Unlive`)

If you need to stop serving traffic entirely (e.g., for maintenance or emergency), you can take down the live build.

1.  Run `smallestai agent builds`.
2.  Select the currently **LIVE** build (marked with green).
3.  Choose **Make Unlive**.

```text Terminal Output
? Select a build to manage:
 > bld_xyz789 | SUCCEEDED | LIVE | 2024-01-01 12:00:00

? What would you like to do with build bld_xyz789...?
 > Make Unlive
   Cancel
```
