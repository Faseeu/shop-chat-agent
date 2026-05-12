# Module 5: Security and Customer Data

Handling private customer data (like orders and addresses) requires the highest level of security.

## The Challenge

How do we let an AI Agent access a customer's private data without the merchant (the app owner) ever seeing the customer's password?

## The Solution: PKCE OAuth

We use the **Proof Key for Code Exchange (PKCE)** flow. This is a secure way to perform OAuth in environments where you can't securely store a client secret (like mobile apps or, in our case, to add an extra layer of security to the AI conversation).

### The Step-by-Step Flow:

1.  **Authorization Request**: The app generates a `code_verifier` and a `code_challenge`.
2.  **Redirect**: The user is sent to Shopify with the `code_challenge`.
3.  **Callback**: Shopify sends back a `code`.
4.  **Token Exchange**: The app sends the `code` AND the original `code_verifier`. Shopify verifies that the challenge matches the verifier.
5.  **Success**: The app receives an `access_token`.

## Implementation in the App

- **`app/auth.server.js`**: Contains the logic for generating the PKCE parameters.
- **`app/db.server.js`**: Manages the persistence of `CodeVerifiers` and `CustomerTokens`.
- **`app/routes/auth.callback.jsx`**: The endpoint that receives the code and performs the final exchange.

## Token Scopes

The token we request uses the scope `customer-account-mcp-api:full`. This specifically grants the app permission to call MCP tools on behalf of the customer.

## Data Isolation

Each `CustomerToken` is tied to a `conversationId`. This ensures that even if a user starts multiple chats, their data is managed within the context of that specific session.
