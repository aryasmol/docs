---
title: "Getting Started"
description: "From zero to a running AI agent in 5 minutes."
---

This guide will walk you through installing the SDK, writing your first intelligent agent, and running it locally.

## 1. Installation

First, install the Atoms SDK using pip.

```bash
pip install smallestai
```

## 2. Write Your First Agent

We will create a simple "Echo Agent" that uses OpenAI to generate responses. We need two files: one for the agent logic, and one to run the server.

<Steps>
  <Step title="Create my_agent.py">
    This file defines *what* your agent does. We use `OutputAgentNode` to create a standard conversational agent.

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
    This file runs your agent as a service.

    ```python server.py
    from smallestai.atoms.agent.server import AtomsApp
    from smallestai.atoms.agent.session import AgentSession
    from my_agent import MyAgent

    async def on_start(session: AgentSession):
        # Add our agent to the session
        session.add_node(MyAgent())
        # Start the processing loop
        await session.start()
        
        # Keep the session alive
        await session.wait_until_complete()

    if __name__ == "__main__":
        app = AtomsApp(setup_handler=on_start)
        app.run()
    ```

    > **Why do I need a server?**
    > Unlike a simple script that runs once and exits, conversational agents need to maintain a connection. `server.py` starts a **WebSocket server** that handles real-time, bidirectional audio or text streams. This enables your agent to listen and speak simultaneously (interruptibility) and maintain state across a long conversation.
  </Step>
</Steps>

## 3. Initialize & Go Live

To make your agent accessible to the world (or your frontend), deploy it to the Atoms platform.

<Steps>
  <Step title="Initialize">
    Link your directory to a project.
    ```bash
    smallestai agent init
    ```
  </Step>

  <Step title="Deploy">
    Push your code to the cloud.
    ```bash
    smallestai agent deploy
    ```
  </Step>

  <Step title="Make Live">
    1. Run `smallestai agent builds`
    2. Select your latest build and choose **Make Live**
  </Step>
</Steps>

## 4. Run Locally

You can also run your agent locally for testing.

```bash
python server.py
```
This starts the WebSocket server on `http://0.0.0.0:8080`.

## 5. Chat

Use the CLI to chat with your running agent.

```bash
smallestai agent chat
```

## What's Next?

Now that you have a running agent, explore **Cookbooks** to see what else you can build.

<CardGroup cols={2}>
  <Card title="Tool Calling" icon="toolbox" href="/cookbooks/tools">
    Give your agent calculators, search, and APIs.
  </Card>
  <Card title="Orchestration" icon="diagram-project" href="/cookbooks/orchestration">
    Connect multiple agents for complex workflows.
  </Card>
</CardGroup>
