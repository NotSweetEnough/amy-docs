# Security

## Model

Amy's extension contains no credentials and no backend address. Each organization connects its own backend and keeps its AI provider credentials in its own secret store. The browser never receives, stores, displays or logs an AI credential.

| Boundary | Control |
| --- | --- |
| Extension → organization configuration | Non-secret document in Azure DevOps extension data. Strict schema with unknown fields rejected; URL, HTTPS and trusted-hostname checks on every load; change history. Storage cannot restrict writers (see [ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md)), so every user confirms the backend hostname before first use and after every change. |
| Extension → backend | HTTPS. Metadata check without following redirects. Entra ID sign-in run by the backend; the session token is kept in the browser tab's `sessionStorage` until it expires (never `localStorage` or cookies), and discarded when the backend rejects it. `Authorization` and `x-azdo-organization` headers; no cookies (`credentials: omit`). |
| Backend authentication | Session token (HS256, `SESSION_SIGNING_KEY`): signature, issuer, **audience (this API only)**, expiry and required claims. At sign-in: ID token RS256 signature against the published keys, issuer, audience (the app's client ID), expiry, nonce, PKCE. |
| Backend authorization | Allowed tenants; allowed Azure DevOps organizations (membership verified with Azure DevOps at sign-in, and the header must match the session); optional allowed users, groups or app role; enabled operations. A valid token alone is not enough. CORS is not used as authentication. |
| Organization isolation | Usage counters, rate limits, concurrency, audit records and sign-in state are keyed by organization. The admin usage endpoint returns one organization at a time. |
| Backend → secrets | Reference deployment: Key Vault (RBAC) references resolved by the App Service managed identity (Key Vault Secrets User, read-only). No Key Vault SDK; no secrets in IaC, pipelines or outputs. |
| Backend → AI provider | HTTPS in production. The key is injected from validated configuration. Timeout and bounded retries. Upstream bodies are never logged or returned. |
| Extension → Azure DevOps | User identity and documented scopes ([PERMISSIONS.md](PERMISSIONS.md)). No PAT. Writes only after explicit confirmation. |
| Service hook → backend | Shared secret in the `x-amy-hook-secret` header (compared in constant time, stored in Key Vault). Organization taken from the payload and checked against `ALLOWED_ORGANIZATIONS`. Completion is verified by reading the pull request again, never from the payload. Deliveries are deduplicated by event ID. |
| Backend → Azure DevOps (pull-request checks) | The App Service managed identity, with read access to the enabled repositories and Wikis and permission to comment on pull requests. It never writes Wiki pages or work items. |
| MCP client → backend | Entra access token for this API (v2, RS256 against the published keys, issuer, audience, expiry, delegated scope `Amy.Mcp`), then the same tenant, organization, user, group and role policy as sessions. Azure DevOps is read with an on-behalf-of token for the user; no backend identity is used. Origins outside `ALLOWED_ORIGINS` are refused. |
| Stored proposals | Keyed by organization. AI-client proposals are returned and decided only for their owner. A decision applies only to the revision the user reviewed (`409` otherwise). |

## Reference backend controls

- **Fail-fast configuration:** startup fails on missing values, placeholders, non-HTTPS production URLs, credentials in URLs, unresolved Key Vault references, `AUTH_MODE=none` in production, or missing `ALLOWED_ORGANIZATIONS`. Messages name the variable, never its value.
- **No secrets in responses:** health, readiness and metadata expose no secrets, tenant IDs, provider names or infrastructure names, and probes never call the provider.
- **Logging:**
  - **Logged:** metadata only (correlation ID, operation, organization, pseudonymous user reference, latency, outcome, status category, retries, token counts).
  - **Never logged:** request query strings (sign-in codes) and client addresses.
  - **Redacted at any depth:** `authorization`, `cookie`, `api-key`, `x-api-key`, `x-admin-token`, `AI_PROVIDER_API_KEY`, `ENTRA_CLIENT_SECRET`, `SESSION_SIGNING_KEY`, `ADMIN_DIAGNOSTICS_TOKEN`, `PR_DOCS_HOOK_SECRET`, `ADO_PAT`, `client_secret`, `access_token`, `refresh_token`, `id_token`, `code_verifier` and similar.
  - **Errors** are serialized without headers.
- **Usage protection:** per-user, per-organization and global request limits, daily and monthly token quotas, rate and concurrency limits, input and output-token limits, enabled operations, and an emergency switch. Automatic pull-request checks and MCP calls go through the same limits.
- **Model output:** schema and semantic validation with one repair attempt. Output is escaped before it is written to Azure DevOps.

## Reporting a vulnerability

Report vulnerabilities privately on GitHub: [Report a vulnerability](https://github.com/NotSweetEnough/amy-docs/security/advisories/new). Only the publisher sees the report. Don't open a public issue, and don't include credentials or content from your organization.

## Review checklist (backend operators)

- [ ] `AUTH_MODE=entra`, with `ALLOWED_ORGANIZATIONS` and `ALLOWED_ORIGINS` limited to your organizations and the Marketplace iframe origin.
- [ ] `ENTRA_CLIENT_SECRET`, `SESSION_SIGNING_KEY`, `AI_PROVIDER_API_KEY` (and `ADMIN_DIAGNOSTICS_TOKEN`) are Key Vault references, shown as resolved in the portal.
- [ ] The app identity has only Key Vault Secrets User; secret administrators are separate.
- [ ] User consent to unverified applications is restricted in your Entra tenant.
- [ ] Usage limits match your budget; `AI_DISABLED` is known to on-call staff.
- [ ] The data handling ([DATA-HANDLING.md](DATA-HANDLING.md)) matches your AI provider's terms.
