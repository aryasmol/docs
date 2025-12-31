---
title: "Memory & State"
description: "Managing state across the conversation."
---

In the Atoms SDK, an instance of your Agent Node is created for each session. This means you can simply use **instance attributes** to maintain state for the duration of that conversation.

## Session State

Store user details, flags, or conversation context directly on `self`.

```python
class RegistrationAgent(OutputAgentNode):
    def __init__(self):
        super().__init__(name="registration_agent")
        # Initialize state
        self.user_data = {}
        self.step = 0

    async def process_user_input(self, text: str):
        if self.step == 0:
            self.user_data["name"] = text
            self.step += 1
            await self.speak(f"Nice to meet you, {text}. What is your email?")
        elif self.step == 1:
            self.user_data["email"] = text
            await self.speak("Thanks! Registration complete.")
```

## Accessing Context

You can access the conversation history through `self.context`.

```python
# Get all messages in the current session
history = self.context.messages

# Add a message manually (e.g. specialized system prompt updates)
self.context.add_message({
    "role": "system",
    "content": "Now acting as a support agent."
})
```
