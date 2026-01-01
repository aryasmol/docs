---
title: "Getting Started"
description: "From zero to a running AI agent in 5 minutes."
---

This guide will walk you through installing the SDK, writing your first intelligent agent, and running it.

## 1. Installation

```bash
pip install smallestai
```

## 2. Write Your First Agent

We need two files: one for the agent logic, and one to run the application.

<Steps>
  <Step title="Create my_agent.py">
    This file defines *what* your agent does.

    ```python my_agent.py
    from smallestai.atoms.agent.nodes import OutputAgentNode
    from smallestai.atoms.agent.clients.openai import OpenAIClient

    class MyAgent(OutputAgentNode):
        def __init__(self):
            super().__init__(name="my-agent")
            self.llm = OpenAIClient(model="gpt-4o-mini")

        async def generate_response(self):
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
        session.add_node(MyAgent())
        await session.start()
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

## 3. Run Your Agent

Once your files are ready, you have two options:

<Tabs>
  <Tab title="Run Locally">
    For development and testing, run the file directly:

    ```bash
    python main.py
    ```

    This starts a WebSocket server on `localhost:8080`. In a separate terminal, connect to it:

    ```bash
    smallestai agent chat
    ```

    No account or deployment needed.
  </Tab>

  <Tab title="Deploy to Platform">
    To have SmallestAI host your agent in the cloud (for production, API access, or phone calls):

    <Steps>
      <Step title="Login">
        ```bash
        smallestai auth login
        ```
      </Step>
      <Step title="Initialize">
        Link your directory to an agent on the platform.
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
        Run `smallestai agent builds`, select your build, and choose **Make Live**.
      </Step>
    </Steps>
  </Tab>
</Tabs>

## What's Next?

<CardGroup cols={2}>
  <Card title="Tool Calling" icon="toolbox" href="/cookbooks/tools">
    Give your agent calculators, search, and APIs.
  </Card>
  <Card title="Orchestration" icon="diagram-project" href="/cookbooks/orchestration">
    Connect multiple agents for complex workflows.
  </Card>
</CardGroup>
