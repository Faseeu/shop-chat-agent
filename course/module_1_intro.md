# Module 1: Introduction to AI Storefront Agents and MCP

## What is an AI Storefront Agent?

An AI Storefront Agent is more than just a chatbot. While traditional chatbots follow rigid decision trees, an Agent uses a Large Language Model (LLM) to:
- **Understand Intent**: Interpret natural language queries like "I need something for a snowy hike."
- **Reason**: Determine which actions are needed to fulfill a request.
- **Act**: Use "tools" to search catalogs, manage carts, or check order status.

## Introduction to MCP

The **Model Context Protocol (MCP)** is an open standard that enables developers to provide context and tools to LLMs in a standardized way.

### Why MCP?
Previously, connecting an LLM to an API required writing custom "glue code" for every single tool. With MCP:
1.  **Servers** expose tools via a standard protocol.
2.  **Clients** (like our app) connect to these servers.
3.  **LLMs** consume the tool definitions and decide when to call them.

## Shopify and MCP

Shopify has embraced MCP by providing managed MCP servers that expose the Storefront API and Customer Account API as tools. This means your AI Agent can instantly "know" how to:
- Search products.
- Read store policies.
- Check customer orders.
- Manage shopping carts.

By the end of this course, you will understand how to orchestrate these tools to create a seamless shopping experience.
