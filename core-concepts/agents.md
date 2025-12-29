---
title: "Agents"
description: "LLM-powered nodes that drive the conversation."
---

**Agents** are specialized Nodes that integrate with Large Language Models (LLMs). They extend the capabilities of a standard Node to include prompt management, tool execution, and response streaming.

## Types of Agents

<Tabs>
  <Tab title="OutputAgentNode">
    **User-Facing**
    Designed to communicate directly with the end user. It streams tokens back to the client as they are generated.
    
    *Use case: The primary conversational persona.*
  </Tab>
  <Tab title="BackgroundAgentNode">
    **Internal**
    Runs silently in the background. It processes data or makes decisions without sending text directly to the user.
    
    *Use case: Intent classification, sentiment analysis, data extraction.*
  </Tab>
</Tabs>

## Creating an Agent

To create an agent, inherit from `OutputAgentNode` or `BackgroundAgentNode` and implement `generate_response`.

```python
from smallestai.atoms.agent.nodes import OutputAgentNode

class SalesAgent(OutputAgentNode):
    async def generate_response(self):
        # Your logic here to call LLM
        pass
```

## Tools

Agents can be equipped with tools to perform actions. See the [Tool Usage](/patterns/tools) pattern for details on how to register functions as tools.
