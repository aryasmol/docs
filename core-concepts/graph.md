---
title: "Graph"
description: "How data flows through your system."
---

The **Graph** is the directed network of nodes that defines your application logic. Unlike a flat list of handlers, a graph allows for complex, non-linear workflows.

## Anatomy of a Graph

<CardGroup cols={3}>
  <Card title="Root Nodes" icon="arrow-down">
    The entry points. They receive events directly from the Session (e.g., User Input).
  </Card>
  <Card title="Processing Nodes" icon="microchip">
    Intermediate nodes that transform data, make decisions, or filter events.
  </Card>
  <Card title="Leaf Nodes" icon="arrow-up">
    The end points. They produce final output actions (usually Output Agents).
  </Card>
</CardGroup>

## Data Flow

Events flow strictly **downstream** from parents to children.

1.  **Ingest**: `Session` receives data → `Root Node` queue.
2.  **Process**: `Root Node` processes it → emits new event.
3.  **Propagate**: New event is pushed to `Child Node` queue.
4.  **Repeat**: This continues until a node produces no output or the end of the graph is reached.

<Note>
  Cycles are generally avoided to prevent infinite event loops, though specialized 'loop' nodes are possible.
</Note>
