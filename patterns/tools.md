---
title: "Tools"
description: "Equipping agents with external capabilities."
---

Tools bridge the gap between your agent's reasoning "brain" and the real world. They allow agents to fetch data, perform calculations, or trigger external APIs.

## The Tooling Workflow

<Steps>
  <Step title="Define the Tool">
    Use the `@function_tool` decorator to transform any Python method into an LLM-compatible tool.
    
    <Warning>
      **Docstrings are Critical**: The LLM reads your docstring and type hints to understand *how* and *when* to use the tool. Be descriptive!
    </Warning>

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
  </Step>

  <Step title="Register the Registry">
    Initialize the `ToolRegistry` and let it auto-discover your decorated methods.

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
  </Step>

  <Step title="Handle Execution">
    When the LLM decides to use a tool, it emits a `ToolCall`. You simply pass this to the registry to execute.

    ```python
    if tool_calls:
        results = await self.tool_registry.execute(tool_calls, parallel=True)
        # Add results back to context so the LLM sees them
        self.context.add_tool_results(results)
    ```
  </Step>
</Steps>
