# Guide: Multi-Store Deployment for Agencies

If you are an agency or a SaaS provider looking to deploy this AI Agent for multiple Shopify merchants, you need a strategy for scaling and multi-tenancy.

## 1. Multi-Tenancy Architecture

The current template is designed for a single app instance. To support multiple stores, you have two options:

### Option A: Independent Deployments (The "Pod" Model)
Deploy a separate instance of the app (e.g., on different Render/Heroku/Cloudflare apps) for each client.
- **Pros**: Maximum isolation, dedicated resources, custom codebases per client.
- **Cons**: High maintenance overhead, higher costs.

### Option B: Shared Multi-Tenant Backend (The "SaaS" Model)
Use a single deployment to handle multiple Shopify stores.
- **The Database**: Ensure your `Session` table (managed by Prisma) is correctly storing `shop` domains. All other tables (`Conversation`, `Message`, `CustomerToken`) are already linked to IDs that can be mapped back to a specific shop.
- **Environment Variables**: You must handle `SHOPIFY_API_KEY` and `SHOPIFY_API_SECRET` dynamically or use a single "Public" Shopify App that is installed on many stores.
- **Isolation**: Add a `shopId` column to every table to ensure data for "Store A" is never leaked to "Store B".

## 2. Shared vs. Dedicated LLM Keys

- **Merchant-Provided Keys**: You can modify the app settings to allow merchants to input their own `CLAUDE_API_KEY`.
- **Agency-Provided Keys**: You provide the key and bill the merchant based on usage (tokens used). In this case, you must implement strict rate limiting per store.

## 3. Configuration Management

To manage different stores efficiently, create a "Merchant Dashboard" within the app where you can:
1.  **Configure Prompts**: Each store needs a unique voice and knowledge base.
2.  **Toggle Tools**: Some stores may want the Cart tool, others may only want the FAQ tool.
3.  **Monitor Conversations**: A view for the merchant to see what their customers are asking.

## 4. Deployment Pipeline

1.  **CI/CD**: Set up a pipeline (e.g., GitHub Actions) that automatically deploys changes to your production environment.
2.  **Database Migrations**: Be extremely careful with `prisma migrate` in a multi-tenant environment. Always use a staging database first.
3.  **Tunneling**: For production, move away from local tunnels and use a stable HTTPS URL configured in your Shopify App settings.
