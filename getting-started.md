---
title: "Getting Started"
description: "From zero to a running AI agent in 5 minutes."
---

This guide will walk you through installing the SDK, writing your first intelligent agent, and running it locally.

## 1. Installation

First, install the Atoms SDK using pip. Usage of a virtual environment is recommended.

```bash
pip install smallestai
```

## 2. Write Your First Agent

We will create a simple "Echo Agent" that uses OpenAI to generate responses.

<Steps>
  <Step title="Create my_agent.py">
    This file defines the agent's behavior. We inherit from `OutputAgentNode` to create a user-facing agent.

    ```python my_agent.py
    from smallestai.atoms.agent.nodes import OutputAgentNode
    from smallestai.atoms.agent.clients.openai import OpenAIClient

    class MyAgent(OutputAgentNode):
        def __init__(self):
            super().__init__(name="my-agent")
            # Initialize OpenAI client (requires OPENAI_API_KEY env var)
            self.llm = OpenAIClient(model="gpt-4o-mini")

        async def generate_response(self):
            # Stream the response from the LLM based on conversation context
            async for chunk in await self.llm.chat(self.context.messages, stream=True):
                if chunk.content:
                    yield chunk.content
    ```
  </Step>

  <Step title="Create server.py">
    This file sets up the session and runs the server.

    ```python server.py
    from smallestai.atoms.agent.server import AtomsApp
    from smallestai.atoms.agent.session import AgentSession
    from my_agent import MyAgent

    async def on_start(session: AgentSession):
        # Add our agent to the session
        session.add_node(MyAgent())
        # Start the processing loop
        await session.start()
        
        # Wait for the session to finish
        await session.wait_until_complete()

    if __name__ == "__main__":
        # Create and run the server
        app = AtomsApp(setup_handler=on_start)
        app.run()
    ```
  </Step>
</Steps>

## 3. Initialize & Go Live

Before you can chat, you must link your code to an Atoms agent and make it live on the platform.

<Steps>
  <Step title="Initialize">
    Link your directory to an agent.
    ```bash
    smallestai agent init
    ```
  </Step>

  <Step title="Deploy">
    Deploy the agent to atoms infra.
    ```bash
    smallestai agent deploy
    ```
  </Step>

  <Step title="Make Live">
    Promote your build to serve traffic.
    1. Run `smallestai agent builds`
    2. Select your new build
    3. Choose **Make Live**
  </Step>
</Steps>

## 4. Run It

The Atoms CLI is used for management (deploy, auth), but to run your agent locally, you execute the server file directly.

```bash
python server.py
```

You should see logs indicating the server is running on `http://0.0.0.0:8080`.

## 5. Test It

You can now connect to your agent using the CLI chat tool.

```bash
smallestai agent chat
```

## What's Next?

Now that you have a running agent, explore common **Patterns** to add more power.

<CardGroup cols={2}>
  <Card title="Tool Calling" icon="toolbox" href="/patterns/tools">
    Give your agent calculators, search, and APIs.
  </Card>
  <Card title="Orchestration" icon="diagram-project" href="/patterns/orchestration">
    Connect multiple agents for complex workflows.
  </Card>
</CardGroup>
