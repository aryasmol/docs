---
title: "Nodes"
description: "The building blocks of the Atoms graph."
---

## What is a Node?

A **Node** is a stateful event processor that can have parents and children, forming a directed graph. In the Atoms SDK, everything that processes an event is a Node.

### Key Characteristics

*   **Unique Name**: Every node has a distinct identifier within the graph.
*   **Event Queue**: Nodes maintain their own queue of incoming events to process.
*   **Asynchronous**: Event processing happens non-blockingly.
*   **Topology**: Nodes can be connected to multiple parents and multiple children.

### In Code

All nodes inherit from the base `Node` class found in `smallestai.atoms.agent.nodes.base`.

```python
from smallestai.atoms.agent.nodes.base import Node

class MyCustomNode(Node):
    async def process_event(self, event: SDKEvent):
        # specific logic here
        await super().process_event(event)
```
