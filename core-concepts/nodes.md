---
title: "Nodes"
description: "The fundamental units of processing in an Atoms graph."
---

A **Node** is a stateful entity that processes events. It is the base class for everything in your agent graph, including Agents.

## Capabilities

All nodes share a common set of capabilities:

<CardGroup cols={2}>
    <Card title="Event Queue" icon="layer-group">
        Each node has its own async queue for processing events sequentially.
    </Card>
    <Card title="State Management" icon="database">
        Nodes can maintain state across the lifetime of a session.
    </Card>
    <Card title="Context" icon="book-open">
        Access to shared session context and history.
    </Card>
    <Card title="Relationships" icon="share-nodes">
        Nodes know their parents and children in the DAG.
    </Card>
</CardGroup>

## The Node Interface

Every node implements the `BaseNode` interface.

```python
class Node:
    async def process_event(self, event: SDKEvent):
        """Process an incoming event."""
        pass

    async def send_event(self, event: SDKEvent):
        """Send an event to children nodes."""
        pass
```


## Lifecycle

1.  **Initialization**: Node is created and configured.
2.  **Registration**: Node is added to a `Session`.
3.  **Processing**: Node receives events via `process_event`.
4.  **Teardown**: Session ends, node cleans up resources.
