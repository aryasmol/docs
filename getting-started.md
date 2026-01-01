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

We will create a simple agent that uses OpenAI to generate responses. We need two files: one for the agent logic, and one to run the application.

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

  <Step title="Create main.py">
    This file is the entry point that runs your agent. It handles real-time, bidirectional streams so your agent can listen and respond simultaneously.

    ```python main.py
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

    <Tip>
      Your entry point can be named anything (`app.py`, `run.py`, etc.). When deploying, specify it with `--entry-point your_file.py`.
    </Tip>
  </Step>
</Steps>

## 3. Initialize & Deploy

To deploy your agent to the cloud, link your directory to a project and push your code.

<Steps>
  <Step title="Initialize">
    Link your directory to a project on the platform.
    ```bash
    smallestai agent init
    ```
  </Step>

  <Step title="Deploy">
    Push your code to the cloud.
    ```bash
    smallestai agent deploy --entry-point main.py
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
python main.py
```

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
