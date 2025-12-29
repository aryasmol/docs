---
title: "Calls"
description: "Retrieve call history and transcripts."
---

The `CallsApi` provides access to your agent's call history.

## Clients

```python
from smallestai.atoms.api_client import ApiClient
from smallestai.atoms.api.calls_api import CallsApi

client = ApiClient()
api = CallsApi(client)
```

## Operations

### List Calls

Fetch a list of recent calls.

```python
calls = api.calls_get(limit=10)
for call in calls.items:
    print(f"Call ID: {call.id} | Status: {call.status}")
```

### Get Call Details

Get detailed information, including transcripts, for a specific call.

```python
call_details = api.calls_id_get(id="call-id")
print(f"Duration: {call_details.duration_seconds}s")
print(f"Transcript: {call_details.transcript}")
```
