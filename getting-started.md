---
title: "Getting Started"
description: "Get up and running with your first Agent."
---

## Installation

Install the Smallest AI SDK using pip:

```bash
pip install smallestai
```

You will also need to set your OpenAI API key:

```bash
export OPENAI_API_KEY=sk_...
```

## Quick Start Guide

Let's build a simple "Echo Agent" that listens to user input and responds using an LLM.

### 1. Create the Agent

Create a file named `my_agent.py`:

```python
# my_agent.py
from smallestai.atoms.agent.nodes import OutputAgentNode
from smallestai.atoms.agent.clients.openai import OpenAIClient

class MyAgent(OutputAgentNode):
    def __init__(self):
        super().__init__(name="my-agent")
        # Initialize the OpenAI client with a model
        self.llm = OpenAIClient(model="gpt-4o-mini")

    async def generate_response(self):
        # Stream the response from the LLM based on the current context
        # self.context.messages provides access to the conversation history
        async for chunk in await self.llm.chat(self.context.messages, stream=True):
            if chunk.content:
                yield chunk.content
```

### 2. Create the Server

Create a file named `server.py` to run your agent in a session:

```python
# server.py
from smallestai.atoms.agent.server import AtomsApp
from smallestai.atoms.agent.session import AgentSession
from my_agent import MyAgent

async def setup(session: AgentSession):
    # Add the agent node to the session
    session.add_node(MyAgent())
    # Start the session
    await session.start()

if __name__ == "__main__":
    # Initialize the AtomsApp with the setup handler
    app = AtomsApp(setup_handler=setup)
    app.run(port=8080)
```

### 3. Run It

Run your server:

```bash
python server.py
```

Your agent is now listening for WebSocket connections on port 8080!
