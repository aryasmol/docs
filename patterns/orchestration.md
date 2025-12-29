---
title: "Multi-Agent Patterns"
description: "Architectural recipes for connecting multiple agents."
---

Building a single agent is powerful, but connecting multiple specialized agents creates a system that is greater than the sum of its parts. This page explores common patterns for orchestrating these connections.

<CardGroup cols={2}>
  <Card title="The Router" icon="signs-post">
    A central node analyzes intent and directs traffic to the right specialist.
  </Card>
  <Card title="The Handoff" icon="handshake">
    One agent explicitly passes control to another when it hits a boundary.
  </Card>
</CardGroup>

## Implementation Patterns

<Tabs>
  <Tab title="The Router Pattern">
    In this pattern, a lightweight "Router Node" sits at the front. It doesn't handle the conversation itself; it purely analyzes the signal to decide *who* should handle it.

    **Why use it?**
    *   Keeps specialist prompts clean and focused.
    *   Reduces latency by avoiding heavy LLM calls for simple routing.
    
    ```mermaid
    graph LR;
        User-->Router;
        Router-->Sales;
        Router-->Support;
    ```

    ### Implementation

    ```python
    class RouterNode(BaseNode):
        async def process_event(self, event):
            # 1. Analyze user intent (e.g., using a lightweight LLM or classifier)
            intent = analyze_intent(event.content)
            
            # 2. Add destination to metadata
            if intent == "sales":
                event.metadata["destination"] = "sales_agent"
            elif intent == "support":
                event.metadata["destination"] = "support_agent"
                
            # 3. Forward to all children (agents filter by destination)
            await self.send_event(event)
    
    class SalesAgent(OutputAgentNode):
        async def process_event(self, event):
            # Only process if routed here
            if event.metadata.get("destination") == "sales_agent":
                await super().process_event(event)
    ```
  </Tab>

  <Tab title="The Handoff Pattern">
    Here, an agent is fully in charge until it realizes it can no longer help. It then "hands off" execution to a peer or supervisor.

    **Why use it?**
    *   Natural escalation flows (e.g., Bot -> Human).
    *   Linear workflows (Qualify -> Schedule -> Close).

    ### Implementation

    ```python
    class TriageAgent(OutputAgentNode):
        async def generate_response(self):
            if "talk to human" in user_message:
                # Emit a specialized event to switch context
                await self.send_event(HandoffEvent(target="human_escalation"))
                return
                
            # Normal processing...
    ```
  </Tab>
</Tabs>
