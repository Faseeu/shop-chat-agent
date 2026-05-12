# Module 4: The Frontend - Building the Widget

The storefront experience is powered by a **Shopify Theme Extension**. This allows the chat widget to work on any Shopify theme without manually editing the theme's code.

## 1. App Blocks

The `extensions/chat-bubble/blocks/chat-interface.liquid` file defines an "App Block". Merchants can drag and drop this block into their theme using the Shopify Theme Editor.

## 2. Liquid Integration

We use Liquid to safely pass data from Shopify to our JavaScript:
```liquid
<script>
  window.ShopChatConfig = {
    shopDomain: "{{ shop.permanent_domain }}",
    apiUrl: "https://your-app-url.com",
  };
</script>
```

## 3. The SSE Event Loop in JavaScript

In `chat.js`, we handle the incoming stream from the backend. Since we need to support `POST` requests (to send the user's message), we use a `fetch` request that reads the response body as a stream, rather than the standard `EventSource` (which only supports `GET`).

```javascript
const response = await fetch(url, { ... });
const reader = response.body.getReader();
// ... loop to read chunks and parse JSON-LD formatted SSE data
```

## 4. UI Components

- **Message Bubbles**: Dynamic creation of HTML elements for user and assistant messages.
- **Product Cards**: When the backend sends a `product_results` event, the frontend iterates through the product data and builds a grid of cards with:
    - Image
    - Title
    - Price
    - "View Product" button
- **Typing Indicator**: A simple visual cue that the AI is processing.

## 5. Persistence

We store the `conversation_id` in `localStorage`. This allows a customer to navigate between different pages on the store (e.g., from the Home page to a Product page) and keep their chat history intact.
