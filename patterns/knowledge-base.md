---
title: "Knowledge Base"
description: "Connecting your agent to external knowledge."
---

You can manage a Knowledge Base (RAG) using the `KnowledgeBaseApi` client.

## Setup

First, initialize the API client.

```python
from smallestai.atoms.api_client import ApiClient
from smallestai.atoms.api.knowledge_base_api import KnowledgeBaseApi

# Configure the client (uses defaults or env vars)
api_client = ApiClient()
kb_api = KnowledgeBaseApi(api_client)
```

## Managing Knowledge

You can programmatically list and manage knowledge bases.

```python
# List all knowledge bases
response = kb_api.knowledgebase_get()
for kb in response.knowledgebases:
    print(f"ID: {kb.id}, Name: {kb.name}")

# Delete a knowledge base
# kb_api.knowledgebase_id_delete(id="kb_12345")
```

## RAG Integration

To let your agent use this knowledge, you typically attach the Knowledge Base ID to your agent's configuration or use a tool that queries it.

*(Note: Direct runtime querying from the specialized `OutputAgentNode` is often handled automatically if configured in the platform console, but you can build custom tools to query specific endpoints if needed.)*
