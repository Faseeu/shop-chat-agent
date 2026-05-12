# Module 6: Extending and Customizing

Once you understand the basics, you can tailor the agent to specific brand needs.

## 1. Modifying the System Prompt

The "personality" and "rules" of the agent are defined in `app/prompts/prompts.json`.
You can change the `content` to:
- Change the tone (e.g., "be very formal" vs "be quirky").
- Add specific business rules (e.g., "never mention competitors").
- Provide additional context about the brand.

## 2. Changing the LLM

While this template uses Claude 3.5 Sonnet, you can swap it for other models by modifying `app/services/config.server.js` and updating the `createClaudeService` logic.

*Note: Since we use the Anthropic SDK's streaming features, swapping to a different provider (like OpenAI) would require updating the event handling logic in `claude.server.js`.*

## 3. Adding Custom MCP Tools

The beauty of MCP is its extensibility. You aren't limited to Shopify's tools. You could:
1.  Build your own MCP server (e.g., connecting to a custom ERP or a third-party loyalty program).
2.  Add a new endpoint to the `MCPClient` to connect to your custom server.
3.  Pass these new tools to Claude.

Claude will then be able to orchestrate calls between Shopify's tools and your custom tools seamlessly.

## 4. UI Customization

The chat widget is just HTML, CSS, and JS.
- Modify `chat.css` to match a brand's style guide.
- Update `chat-interface.liquid` to add new UI elements (like a "Suggested Questions" section).
- Enhance `chat.js` to handle new types of data returned by your custom tools.
