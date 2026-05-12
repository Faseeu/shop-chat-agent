# Module 2: Architecture and Setup

Understanding how the pieces fit together is crucial before you start coding.

## The Three-Tier Architecture

1.  **Shopify Managed MCP Servers**: These are "Black Boxes" provided by Shopify. They contain the logic to talk to Shopify's internal APIs.
2.  **Your App (The Backend)**: This is the "Glue". It hosts the LLM, connects to the MCP servers, and manages the user's session.
3.  **The Storefront (The Frontend)**: This is the "Interface". It's where the customer sees the chat bubble and interacts with the AI.

## Environment Variables

To run this app, you need several key pieces of information in your `.env` file:
- `SHOPIFY_API_KEY`: Your app's client ID.
- `SHOPIFY_API_SECRET`: Your app's client secret.
- `CLAUDE_API_KEY`: Your Anthropic API key.
- `REDIRECT_URL`: The callback URL for the customer OAuth flow.
- `SHOPIFY_APP_URL`: The base URL where your app is hosted (e.g., via Cloudflare tunnel).

## Data Persistence

The app uses **Prisma** with **SQLite** by default. This is great for development because it requires zero setup.
- `prisma/schema.prisma`: Defines your tables (Sessions, Conversations, Messages, Tokens).
- `dev.sqlite`: The local database file.

## Getting Started

1.  **Install dependencies**: `npm install`
2.  **Setup the database**: `npm run setup` (This runs `prisma migrate`)
3.  **Start development**: `npm run dev`

`npm run dev` uses the Shopify CLI to:
- Start your local server.
- Create a secure tunnel.
- Update your app configuration in the Shopify Partner Dashboard.
- Provide a link to install the app on your development store.
