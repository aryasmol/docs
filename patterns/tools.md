---
title: "Tools"
description: "Equipping agents with external capabilities."
---

Tools allow agents to perform actions, such as fetching data or performing calculations.

## Defining a Tool

Use the `@function_tool` decorator to turn any Python method into a tool.

```python
from smallestai.atoms.agent.tools.decorator import function_tool

class Assistant(OutputAgentNode):
    
    @function_tool()
    def get_weather(self, location: str):
        """
        Get the current weather for a location.
        
        Args:
            location: The city and state, e.g. "San Francisco, CA"
        """
        # Logic to fetch weather
        return {"temp": 72, "condition": "Sunny"}
```

<Note>
  The docstring and type hints are **critical**. The LLM uses them to understand how and when to call the tool.
</Note>

## Registering Tools

Tools are automatically discovered if they are methods on the Agent class. You just need to initialize the registry.

```python
from smallestai.atoms.agent.tools.registry import ToolRegistry

class Assistant(OutputAgentNode):
    def __init__(self):
        super().__init__(name="assistant")
        self.tool_registry = ToolRegistry()
        # Discover tools defined on 'self'
        self.tool_registry.discover(self)
        
        # Pass schemas to the LLM client
        self.tool_schemas = self.tool_registry.get_schemas()

    async def generate_response(self):
        # Call LLM with tools
        response = await self.llm.chat(
            messages=self.context.messages, 
            stream=True, 
            tools=self.tool_schemas
        )
        # ... Handle tool calls in the response loop ...
```

## Handling Execution

When the LLM decides to use a tool, it emits a `ToolCall`. You execute it using the registry.

```python
if tool_calls:
    results = await self.tool_registry.execute(tool_calls, parallel=True)
    # Add results back to context so the LLM sees them
    self.context.add_tool_results(results)
```
