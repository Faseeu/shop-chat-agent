# Module 3: The Backend - LLMs and Tools

This module dives into the "Brain" of our application.

## 1. The React Router Backend

Our app uses React Router not just for UI, but as a robust Node.js server. The core of our AI logic lives in `app/routes/chat.jsx`.

## 2. Integrating Claude

We use the Anthropic SDK to communicate with Claude.
```javascript
const stream = await anthropic.messages.stream({
  model: 'claude-3-5-sonnet-20240620',
  system: systemInstruction,
  messages,
  tools: mcpClient.tools
});
```
The key here is the `tools` parameter. We pass the tool definitions we received from the Shopify MCP servers directly to Claude.

## 3. The Multi-Step Loop

AI Agents often need multiple steps to solve a problem.
- **User**: "Is my snowboard order shipped?"
- **Claude**: (Calls `get_most_recent_order_status`)
- **Backend**: (Returns order data)
- **Claude**: "Yes, it was shipped yesterday. Here is your tracking number..."

Our backend implements this using a `while` loop that keeps Claude "thinking" and "acting" until it reaches a final answer.

## 4. Streaming with SSE

To provide a snappy UI, we don't wait for the whole response. We use **Server-Sent Events (SSE)** to stream the response chunk-by-chunk.

- **Chunk**: Individual words/characters.
- **Tool Use**: Notification that the agent is "working".
- **Product Results**: Structured data for the UI to render cards.

## 5. The MCP Client Implementation

Our `MCPClient` class in `app/mcp-client.js` is a lightweight wrapper around JSON-RPC. It manages two endpoints:
1.  **Storefront**: Public, no auth required.
2.  **Customer**: Private, requires a Customer Access Token.

It handles the logic of "routing" a tool call request from Claude to the correct Shopify endpoint.
