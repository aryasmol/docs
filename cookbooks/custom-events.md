---
title: "Custom Events"
description: "Signaling between agents with specific events."
---

While standard system events handle core mechanics like text and audio, you often need to signal custom business logic states—like "PaymentFailed" or "EscalationRequired".

<Info>
  **Why Custom Events?**
  Unlike passing untyped dictionaries, defining `SDKEvent` subclasses gives you **type safety**, **validation**, and **clearer intent** in your code.
</Info>

## Implementation

<Steps>
  <Step title="Define the Event">
    Inherit from `SDKEvent` and specify a unique `type` string. This string is used for serialization.

    ```python
    # 1. Define the Event
    class EscalationEvent(SDKEvent, type="escalation_event"):
        reason: str
        severity: Optional[str] = "medium"
    ```
  </Step>

  <Step title="Emit the Event">
    Use `self.send_event()` to broadcast the event to downstream nodes.

    ```python
    class SupportBot(OutputAgentNode):
        async def generate_response(self, messages):
            # ... analysis logic ...
            if "angry" in user_sentiment:
                # Emit the custom event
                await self.send_event(EscalationEvent(
                    reason="User is frustrated",
                    severity="high"
                ))
    ```
  </Step>

  <Step title="Handle the Event">
    Override `process_event` to listen for your custom type.

    ```python
    class SupervisorNode(Node):
        async def process_event(self, event: SDKEvent):
            # Check for your custom event type
            if isinstance(event, EscalationEvent):
                print(f"Escalation received: {event.reason}")
                # Take action...
            
            # Important: Propagate event to children!
            await self.send_event(event)
    ```
  </Step>
</Steps>
