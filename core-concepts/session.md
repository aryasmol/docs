---
title: "Session"
description: "The runtime container for your agent graph."
---

The **Session** (`AgentSession`) interacts with the outside world (the transport layer) and manages the execution of your agents.

## Core Responsibilities

<Steps>
  <Step title="Lifecycle Management">
    Handles the startup and shutdown sequence of the graph and its connections.
  </Step>
  <Step title="Event Routing">
    Takes incoming raw events, validates them, and pushes them into the graph at the Root nodes.
  </Step>
  <Step title="Context Isolation">
    Ensures that data from one user connection does not leak into another. Each connection gets its own `AgentSession` instance.
  </Step>
</Steps>

## Example Usage

```python
from smallestai.atoms.agent.session import AgentSession

# Create a session
session = AgentSession()

# Add nodes/agents
session.add_node(my_agent)

# Start processing
await session.start()
```

## Key Methods

-   `add_node(node)`: Registers a node with the session context.
-   `start()`: Begins the event processing loop.
-   `on_event(event_name)`: Decorator to register callbacks for specific system events.
