---
title: "Introduction"
description: "Build intelligent, multi-agent systems with real-time communication."
---

# What is Atoms SDK?

Atoms SDK enables developers to build conversational AI systems by composing specialized agents in a graph structure. Each agent processes events, communicates with LLMs, and can interact with other agents to create sophisticated workflows.

It is designed for teams that:

*   Need fine-grained control over agent behavior, routing, and tools.
*   Operate in low-latency streaming environments (telephony, web calls, chat widgets).
*   Prefer to keep reasoning and application logic in code, while delegating session orchestration to a runtime.

## Core Philosophy

Atoms SDK is built around the idea of **Agents as Nodes in a Graph**. Instead of a single monolithic agent, you compose clear, single-purpose agents (nodes) that pass messages (events) to each other.

*   **Modular**: Swap out an agent implementation without breaking the rest of the system.
*   **Stateful**: Sessions manage the varying state of a conversation.
*   **Event-Driven**: Everything is an event, from user speech to system errors.
