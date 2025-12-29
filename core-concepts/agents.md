---
title: "Agents"
description: "Specialized nodes for intelligent behavior."
---

## Agents

An **Agent** is a specialized Node that typically uses a Large Language Model (LLM) to process input and generate output.

### OutputAgentNode

Use `OutputAgentNode` when you want to generate responses that are sent back to the user (e.g., text, speech).

*   **Interruptible**: Automatically handles interruption events (e.g., user starts talking while agent is speaking).
*   **Streaming**: Emits `ResponseStart`, `ResponseChunk`, and `ResponseEnd` events standardizing the streaming lifecycle.

```python
from smallestai.atoms.agent.nodes import OutputAgentNode

class MyVoiceAgent(OutputAgentNode):
    # ... implementation ...
```

### BackgroundAgentNode

Use `BackgroundAgentNode` for silent processing, such as:
*   Enriching context (e.g., looking up user data).
*   Analyzing sentiment.
*   Making routing decisions without speaking.
