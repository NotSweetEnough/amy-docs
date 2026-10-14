# Amy AI

Amy AI is an assistant for Azure DevOps Wikis, work items and pull-request documentation. It runs inside Azure DevOps as an extension from the Visual Studio Marketplace.

**Bring your own backend.** Amy includes no hosted AI service, and its publisher operates no backend and receives no data. Each organization runs Amy's backend in its own Azure subscription, with its own AI provider and credentials.

## Get started

1. **Install** Amy AI from the Visual Studio Marketplace into your Azure DevOps organization.
2. **Deploy the backend** in your Azure subscription from the published container image `ghcr.io/notsweetenough/amy-backend`: [DEPLOY.md](DEPLOY.md).
3. **Connect it:** open **Amy** in any project, select the **Amy logo**, then **Backend settings**, enter the backend URL, test the connection and save: [ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md).

Until a backend is connected, the features without AI keep working and the AI features explain what is missing.

## Documentation

| Document | For |
| --- | --- |
| [DEPLOY.md](DEPLOY.md) | Deploying and updating the backend on Azure |
| [AI-PROVIDER-CONFIGURATION.md](AI-PROVIDER-CONFIGURATION.md) | Connecting your AI provider and choosing models |
| [ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md) | Organization administrators: connecting and managing Amy |
| [USER-GUIDE.md](USER-GUIDE.md) | Everyone using Amy |
| [DATA-HANDLING.md](DATA-HANDLING.md) | What is sent where |
| [PERMISSIONS.md](PERMISSIONS.md) | The Azure DevOps permissions Amy requests |
| [SECURITY.md](SECURITY.md) | Security model and how to report a vulnerability |
| [PRIVACY.md](PRIVACY.md) | Privacy statement |
| [LICENSE.md](LICENSE.md) | License terms |
| [SUPPORT.md](SUPPORT.md) | Getting help |
