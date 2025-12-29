---
title: 'Logs'
description: 'Retrieve conversation logs and details.'
---

The `LogsApi` allows you to access detailed logs for specific conversations. This is useful for auditing, debugging, and analyzing agent performance.

## Initialize Logs API

```python
from smallestai.atoms.api import LogsApi

# Initialize the API client
api = LogsApi()
```

## Get Conversation Logs

Retrieve detailed logs for a specific conversation using its `call_id`.

```python
# specific_call_id is a string, usually a UUID
call_id = "your_call_id_here"

try:
    # Get conversation details
    logs = api.conversation_id_get(id=call_id)
    print(f"Logs for call {call_id}: {logs}")
except Exception as e:
    print(f"Error fetching logs: {e}")
```
