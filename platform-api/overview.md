---
title: "Overview"
description: "Programmatically manage your Atoms platform resources."
---

The Platform API allows you to manage agents, calls, campaigns, knowledge bases, and more programmatically.

## The Atoms Client

The `AtomsClient` is the main entry point for interacting with the Smallest.ai Platform API. It wraps all specific API clients (Agents, Calls, etc.) into a single, cohesive interface.

### Initialization

```python
from smallestai.atoms import AtomsClient

# Initialize the client (picks up ATOMS_API_KEY from environment)
client = AtomsClient()
```

### Accessing APIs

You can access specific functionality through the client's properties or convenience methods:

```python
# Create an agent using the convenience method
client.create_agent(create_agent_request=...)

# Or access the specific API directly
client.agents_api.agent_post(...)

# Managing Knowledge Bases
client.get_knowledge_bases()

# Checking Logs
client.get_conversation_logs(id="call_id")
```

## Available APIs

*   [Agents](/platform-api/agents): Manage agent configurations and templates.
*   [Calls](/platform-api/calls): Retrieve call history and transcripts.
*   [Campaigns](/platform-api/campaigns): Create and manage outbound calling campaigns.
*   [Knowledge Base](/patterns/knowledge-base): Manage RAG knowledge bases.
*   [Logs](/platform-api/logs): Access detailed conversation logs.
*   [Organization](/platform-api/organization): View organization details.
*   [User](/platform-api/user): View authenticated user details.
*   [Campaigns](/platform-api/campaigns): Create and manage outbound campaigns.
*   [Logs](/platform-api/logs): Access system logs for debugging.
