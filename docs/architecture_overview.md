# Architecture Overview: Shopify AI Storefront Agent

This project is a sophisticated Shopify application template designed to embed an AI-powered chat assistant directly into a merchant's storefront. It leverages the **Model Context Protocol (MCP)** to bridge the gap between Large Language Models (LLMs) and Shopify's specialized commerce data and actions.

## System Components

The application is comprised of three primary architectural layers:

1.  **React Router Backend (The Brain)**:
    - Acts as an MCP Client.
    - Orchestrates communication between the user, Claude (LLM), and Shopify's MCP servers.
    - Manages conversation state and persistence.
    - Handles secure OAuth flows for customer-specific data.
2.  **Shopify Theme Extension (The Face)**:
    - A "Chat Bubble" app block injected into the storefront.
    - Built using Liquid, CSS, and Vanilla JavaScript.
    - Communicates with the backend via Server-Sent Events (SSE) for real-time, streaming responses.
3.  **Shopify MCP Servers (The Hands)**:
    - Shopify provides two distinct MCP servers:
        - **Storefront MCP**: Provides tools for public data like product search, catalog browsing, and shop policies.
        - **Customer MCP**: Provides tools for private data like cart management, order history, and customer profiles.

## Request Flow

1.  **Initiation**: A shopper interacts with the chat widget on the storefront.
2.  **Message Delivery**: The frontend sends the user message to the `/chat` endpoint on the React Router backend.
3.  **Context Assembly**: The backend retrieves conversation history from the SQLite database and connects to the relevant Shopify MCP servers.
4.  **LLM Interaction**: The backend sends the history, user message, and available MCP tool definitions to Claude.
5.  **Tool Execution**: If Claude needs to perform an action (e.g., search products), it returns a "tool use" request. The backend executes this request via the MCP client and returns the result to Claude.
6.  **Streaming Response**: As Claude generates its final response, the backend streams it back to the storefront widget using SSE.
7.  **Persistence**: Every message and tool result is stored in the database to maintain long-term conversation context.
8.  **UI Rendering**: The chat widget renders the text response and any specialized components (like product cards) returned in the stream.
