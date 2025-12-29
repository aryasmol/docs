---
title: "Custom Events"
description: "Signaling between agents with specific events."
---

You can define custom events to handle internal signaling between nodes.

## Defining a Custom Event

Inherit from `SDKEvent` and specify a unique `type`.

```python
from smallestai.atoms.agent.events import SDKEvent
from typing import Optional

# Define the event with a unique type string
class EscalationEvent(SDKEvent, type="escalation_event"):
    reason: str
    severity: Optional[str] = "medium"
```

## Emitting Events

Use `self.send_event()` to broadcast the event to downstream nodes.

```python
class SupportBot(OutputAgentNode):
    async def generate_response(self):
        if "angry" in user_sentiment:
            # Emit the custom event
            await self.send_event(EscalationEvent(
                reason="User is frustrated",
                severity="high"
            ))
```

## Handling Events

Override `process_event` to listen for your custom type.

```python
class SupervisorNode(BaseNode):
    async def process_event(self, event: SDKEvent):
        # Check for your custom event type
        if isinstance(event, EscalationEvent):
            print(f"Escalation received: {event.reason}")
            # Take action...
        else:
            # Important: Pass through other events!
            await super().process_event(event)
```
