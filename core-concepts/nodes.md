---
title: "Nodes"
description: "The fundamental building blocks of your agent's logic."
---

A **Node** is the basic unit of computation in the Atoms graph. Every "agent" or functional component you build is ultimately a Node.

## What is a Node?

In the conceptual graph, a Node is a vertex that performs three key actions:

<CardGroup cols={3}>
  <Card title="Receives" icon="inbox">
    Accepts incoming events like user audio, text, or system triggers.
  </Card>
  <Card title="Processes" icon="microchip">
    Executes custom Python code, business logic, or AI inference.
  </Card>
  <Card title="Sends" icon="paper-plane">
    Manually emits new events to pass control to the rest of the graph.
  </Card>
</CardGroup>

## Two Types of Nodes

Atoms provides two primary base classes to inherit from, depending on your needs.

### 1. The Output Agent (`OutputAgentNode`)

This is the most common node type. It is a full-featured conversational agent designed to interact with Large Language Models (LLMs).

**Key Features:**
*   **Auto-Interruption**: Automatically handles user interruptions during playback only when the user is speaking.
*   **Streaming**: Managers the complexity of streaming LLM tokens to the user in real-time.
*   **Context Management**: Maintains conversation history automatically.

**Use Case**: The "brain" of your bot—Sales Agent, Support Agent, Triage Agent.

```python
from smallestai.atoms import OutputAgentNode

class MyAgent(OutputAgentNode):
    def __init__(self):
        super().__init__(
            model="gpt-4o",
            system_prompt="You are a helpful assistant."
        )
```

### 2. The Base Node (`Node`)

The `Node` class is the raw primitive. It gives you full control but assumes nothing. It is perfect for deterministic logic, API calls, or routing decisions.

**Key Features:**
*   **Raw Event Access**: You get the raw event and decide exactly what to do with it.
*   **No Overhead**: No LLM context or streaming logic unless you build it.

**Use Case**: Router, API Fetcher, Database Logger, Analytics Tracker.

```python
from smallestai.atoms.agent.nodes import Node

class RouterNode(Node):
    async def process_event(self, event):
        # Deterministic logic
        if "sales" in event.content:
             # Manually send event to the next node
            await self.send_event(event, destination="SalesAgent")
        else:
            await self.send_event(event, destination="SupportAgent")
```

## How to Write a Custom Node

<Steps>
  <Step title="Inherit from Node">
    Create a new class that inherits from `Node` (or `OutputAgentNode`).
    ```python
    class LoggerNode(Node):
    ```
  </Step>
  <Step title="Override process_event">
    Implement the `process_event` async method. This is your logic handler.
    ```python
    async def process_event(self, event):
        print(f"LOG: Received event type {event.type}")
    ```
  </Step>
  <Step title="Propagate Events">
    **Crucial:** You must manually send events if you want the flow to continue.
    ```python
    await self.send_event(event) 
    ```
  </Step>
</Steps>

<Warning>
  **Manual Event Propagation**
  In a custom `Node`, the chain of events stops with you unless you explicitly move it forward. You **MUST** call `await self.send_event(...)` if you want the event to continue causing effects in the graph.
</Warning>
