---
layout: page
title: Extending AI Agents with Custom Research Tools
description: How AI agents use tools through MCP and how to create custom tools for research workflows
importance: 5
category: presentations
redirect: https://dissc-yale.github.io/dissc-agent-tooling/
---

An introduction to how AI agents use tools to accomplish complex tasks, focusing on Anthropic's Model Context Protocol (MCP) as the standardized interface for extending agent capabilities.

**What You'll Learn:**
- **Agent Fundamentals**: How autonomous AI systems use tools for memory, planning, execution, and multi-agent orchestration
- **Model Context Protocol Architecture**: The three components—MCP Host (interface), MCP Client (orchestrator), and MCP Server (tools)
- **Creating Custom Tools**: Building MCP servers that expose domain-specific capabilities to agents
- **Semantic Contracts**: How tool metadata (name, description, input schema) guides agent tool selection and usage
- **Practical Examples**: Working implementations of custom MCP servers for research workflows

**Key Insight:**
Tools should focus on high-level capabilities aligned with user intent rather than thin API wrappers. The "semantic contract" formed by tool metadata determines how effectively agents can leverage new capabilities.

Based on Korinek (2025) "Scenarios for the Transition to Transformative AI" and MCP documentation from Anthropic. Includes hands-on examples for implementing custom research tools.

[View Full Presentation →](https://dissc-yale.github.io/dissc-agent-tooling/)
