# Deep Dive: Backend Analysis

The backend is built on **React Router** (formerly Remix), serving both as a traditional web server and a specialized API gateway for AI interactions.

## 1. Routing and Entry Points

- **`app/routes/chat.jsx`**: The core API endpoint. It handles:
    - `GET` requests for conversation history.
    - `POST` requests for starting a chat session.
    - CORS and SSE header management.
- **`app/routes/auth.*.jsx`**: Handles the Shopify admin authentication and the specialized customer OAuth callback.

## 2. Core Services (`app/services/`)

### `claude.server.js`
This service encapsulates the Anthropic SDK. It is responsible for:
- Initializing the Anthropic client.
- Formatting messages for the LLM.
- Managing the streaming lifecycle (`anthropic.messages.stream`).
- Routing stream events (text, message completion, tool use) to appropriate handlers.

### `mcp-client.js`
A custom implementation of an MCP Client. Unlike standard clients that might use stdio, this client communicates with Shopify's MCP servers via **JSON-RPC over HTTP**.
- **`connectToStorefrontServer()`**: Fetches tools from the public storefront MCP endpoint.
- **`connectToCustomerServer()`**: Fetches tools from the authenticated customer MCP endpoint.
- **`callTool()`**: Dispatches tool execution requests to the correct server.

### `streaming.server.js`
Manages Server-Sent Events (SSE). It provides a `StreamManager` that:
- Enqueues data into a `ReadableStream`.
- Handles backpressure and encoding.
- Provides standardized error reporting during a stream.

### `tool.server.js`
Handles the business logic resulting from tool executions:
- **`handleToolSuccess`**: Processes results, specifically formatting product data for the frontend.
- **`handleToolError`**: Handles authentication requirements (triggering the OAuth flow if needed).

## 3. The Conversation Loop

The most critical logic resides in `app/routes/chat.jsx` within the `handleChatSession` function. It implements a **while-loop** that continues until Claude signals it has finished its turn (`stop_reason === "end_turn"`). This allows for multi-step tool execution where Claude can call a tool, get the result, and then decide to call another tool or respond to the user.

## 4. MCP Tool Integration

The system dynamicially discovers tools. When connecting to Shopify's MCP servers, it receives a list of tool definitions (name, description, input schema). These are passed to Claude, allowing the LLM to understand what actions it can take.

**Example Tools provided by Shopify:**
- `search_shop_catalog`: For product discovery.
- `update_cart`: For adding/removing items.
- `get_most_recent_order_status`: For order tracking.
- `search_shop_policies_and_faqs`: For support queries.
