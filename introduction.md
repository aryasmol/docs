---
title: "What is Atoms SDK?"
description: "Build intelligent, real-time AI agents in Python."
---

The Atoms SDK is a framework for building low-latency, stateful AI applications. It handles the complexity of streaming audio, managing conversation state, and coordinating tools, so you can focus on your agent's logic.

## Why Atoms?

Most LLM frameworks are designed for simple chatbots. Atoms is built for **real-time, voice-first experiences**.

*   **Streaming First**: Built to handle real-time audio and text streams with < 500ms latency.
*   **Interruptible**: Automatically handles user interruptions during playback (voice).
*   **Stateful**: Maintains context across long-running sessions, unlike stateless REST APIs.

## How it Works

The SDK provides a simple, code-first way to structure your application:

*   **Composable Logic**: Break complex behaviors into reusable [Nodes](/core-concepts/nodes).
*   **Event Driven**: Unified [Event](/core-concepts/events) system for messages, signals, and tools.
*   **Python Native**: Write standard Python code; no hidden configuration files or DSLs.

## What Next?

<CardGroup cols={2}>
  <Card title="Get Started" icon="play" href="/getting-started">
    Build your first agent in under 5 minutes.
  </Card>
  <Card title="CLI Reference" icon="terminal" href="/cli/overview">
    Master the command line tools.
  </Card>

  <Card title="Cookbooks" icon="layer-group" href="/cookbooks/orchestration">
    Explore common architectural patterns.
  </Card>
</CardGroup>
