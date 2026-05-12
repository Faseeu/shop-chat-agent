# Deep Dive: Authentication and Security

The app handles two distinct types of authentication:
1.  **Admin Auth**: Standard Shopify App authentication for the merchant to manage the app in the Shopify Admin.
2.  **Customer Auth**: A specialized OAuth flow using **PKCE (Proof Key for Code Exchange)** to allow the AI agent to act on behalf of a logged-in customer.

## The PKCE OAuth Flow

When a user asks a question requiring private data (e.g., "Where is my order?"), the following happens:

1.  **Detection**: The `Customer MCP Server` returns a `401 Unauthorized` or Claude identifies a need for auth.
2.  **Challenge Generation**: `app/auth.server.js` generates a `code_verifier` (a random high-entropy string) and a `code_challenge` (a SHA-256 hash of the verifier).
3.  **State Persistence**: The `code_verifier` and a unique `state` (linking the conversation and shop) are stored in the SQLite database via the `CodeVerifier` model.
4.  **Auth URL**: The backend returns a special `auth_required` message to the frontend containing a link to Shopify's Customer Identity service.
5.  **User Consent**: The user clicks the link, logs in (if necessary), and authorizes the app.
6.  **Callback**: Shopify redirects the user to `app/routes/auth.callback.jsx` with an authorization `code`.
7.  **Exchange**: The backend retrieves the stored `code_verifier` using the `state` from the URL, then sends the `code` and `code_verifier` to Shopify's token endpoint.
8.  **Token Storage**: Upon success, a `CustomerToken` is stored in the database, linked to the `conversationId`.

## Database Schema (`prisma/schema.prisma`)

- **`Session`**: Stores merchant admin sessions.
- **`Conversation`**: The root object for a chat thread.
- **`Message`**: Stores individual messages. `content` is stored as a JSON string to accommodate complex tool results and LLM content blocks.
- **`CustomerToken`**: Stores the OAuth access token for a customer, enabling the agent to make authenticated MCP calls.
- **`CodeVerifier`**: Temporary storage for the PKCE flow.
- **`CustomerAccountUrls`**: Caches the Shopify customer account endpoints (`.well-known` discovery) for a shop.
