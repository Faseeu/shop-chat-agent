# Guide: Configuring for Customer Service

While the template is built for general shopping, you can optimize it specifically for customer service and support.

## 1. Refining the System Prompt

The most effective way to turn the agent into a customer service expert is through the system prompt (`app/prompts/prompts.json`).

### Suggested Support Instructions:
- **Tone**: "You are a helpful, empathetic customer service representative for [Store Name]. Use a professional yet friendly tone."
- **Problem Solving**: "If a user has a complaint, acknowledge their frustration, apologize, and try to find a solution using the available tools."
- **Escalation Path**: "If you cannot solve a problem or the user asks for a human, provide the following contact info: [Support Email/Phone]."
- **Policy Enforcement**: "Always refer to the store's official policies before answering questions about returns or shipping."

## 2. Optimizing the Knowledge Base

The `search_shop_policies_and_faqs` tool is the backbone of customer service.
- **Merchant Tip**: Advise merchants to fill out the "Policies" section in their Shopify Admin (Settings > Policies).
- **FAQ Page**: The tool searches the store's content. Create a dedicated "FAQ" page in Shopify with clear headings and concise answers.

## 3. Handling Order Inquiries

The `get_most_recent_order_status` and `get_order_status` tools are critical.
- **Workflow**: Ensure the agent asks for an order number or email if it doesn't have it.
- **Auth**: Remind users that for security, they must log in to see detailed order history.

## 4. Proactive Support

You can modify the frontend (`chat.js`) to trigger the chat bubble proactively:
- **On Error**: Show the chat bubble if a customer's payment fails.
- **On Delay**: If a customer spends more than 2 minutes on the checkout page, have the agent ask if they have any questions.

## 5. Feedback Loop

Implement a "Was this helpful?" thumbs up/down at the end of a conversation to track agent performance. Store this feedback in the database to help refine the system prompt over time.
