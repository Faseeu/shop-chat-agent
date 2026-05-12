# Deep Dive: Frontend Analysis (Theme Extension)

The storefront interface is implemented as a Shopify Theme Extension, allowing it to be easily added to any Online Store 2.0 theme.

## 1. Structure (`extensions/chat-bubble/`)

- **`blocks/chat-interface.liquid`**: The entry point. It defines the HTML structure and uses Liquid to inject shop-specific variables (like `shop.permanent_domain` and the app's backend URL).
- **`assets/chat.css`**: Provides the styling for the chat bubble, the message window, and specialized components like product cards.
- **`assets/chat.js`**: The main logic driver for the UI.

## 2. Communication Layer

The frontend uses the `EventSource` API (or a fetch-based fallback for POST) to handle **Server-Sent Events (SSE)**.

- **Initialization**: On page load, the script generates or retrieves a `conversation_id` from `localStorage`.
- **Message Sending**: When a user types a message, it is sent via `fetch` to the backend's `/chat` route.
- **Stream Processing**: The JavaScript listens for messages from the server. Message types include:
    - `id`: Confirms the conversation ID.
    - `chunk`: A piece of text from the LLM (for real-time typing effect).
    - `tool_use`: Indicates the agent is performing an action.
    - `product_results`: A structured list of products to display as cards.
    - `auth_required`: Triggers the display of an authentication link.
    - `message_complete`: Signals that a full message block has finished.

## 3. Dynamic UI Updates

The JavaScript is responsible for:
- Appending user and assistant messages to the DOM.
- Auto-scrolling to the bottom of the chat.
- Rendering "Typing..." indicators.
- Transforming structured JSON data (from `product_results`) into interactive HTML cards with images and "View Product" links.

## 4. State Management

Minimal state is kept on the client:
- `conversation_id`: Persisted in `localStorage` to maintain chat history across page refreshes.
- Message history is fetched from the backend on load if a `conversation_id` exists.
