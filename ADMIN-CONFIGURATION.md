# Organization administrator configuration

Amy does not include a hosted AI service. After installing the extension, an Azure DevOps organization administrator connects it to a compatible backend that the organization operates (see [DEPLOY.md](DEPLOY.md)). Until then, users see "AI features are not configured for this Azure DevOps organization" and the non-AI features keep working.

## Configure

1. Open **Amy** in any project, select the **Amy logo** (top left), then **Admin settings > Backend settings**. Admin settings are shown to Project Collection Administrators (direct members, or any member the backend recognizes) only.
2. Enter:
   - **Backend URL:** `https://` only. It must not contain credentials, a query string or a fragment; trailing slashes are removed.
   - **Display name.**
   - **Expected API version:** default `1.0`. Backends at 1.1 or later of the same major version also pass; features added in 1.1 appear when the backend offers them.
   - **Trusted hostnames (optional):** an allow-list policy such as `*.your-organization.example`.
   - **Enable AI features.**
3. Select **Test connection**. Amy calls `GET /api/metadata` without following redirects. It checks the response status, content type, service identifier, API version, required capabilities and that authentication is required.
4. Select **Save configuration**.
   - A backend is enabled only after a successful test.
   - **Unreachable backend:** a backend that cannot be reached can be saved only by ticking the override, and then stays disabled.
   - **Replacing a backend:** needs an explicit confirmation. Every user must then confirm the new backend before data is sent to it.

Only non-secret settings are stored. Never enter API keys, client secrets, tokens or Key Vault values; the extension has no fields for them, and the stored document rejects unknown fields. Provider keys belong in your backend's secret store.

Each change records who made it and when. The last 20 changes are listed under **Change history**.

## Where settings are stored

| Data | Storage | Scope |
| --- | --- | --- |
| Backend URL, display name, enabled, API version, trusted hostnames, change history | Azure DevOps extension data, collection scope | This organization only |
| "I confirm this backend" acknowledgement | Azure DevOps extension data, user scope | Per user |
| Backend session token | Browser memory of the open page | Discarded on reload |
| Project templates | Azure DevOps extension data, collection scope, one document per project | That project; changes need review and approval in **Project Templates** |
| Pull-request check settings (repositories, Wiki scope, reviewers) | The organization's backend | This organization only |

Nothing is stored in `localStorage`, `sessionStorage` or cookies.

## Who can change the configuration: platform limitation

Azure DevOps extension data offers no access control: any user who can run the extension can technically write collection-scoped extension data with their own credentials. The admin page shows controls only to direct members of **Project Collection Administrators**, but that is a convenience, not access control.

Amy protects users with checks that do not depend on who wrote the configuration:

1. **User confirmation.** Before any content is sent, each user sees the backend's name and hostname, plus who configured it and when. They must confirm it the first time and after every change.
2. **The backend authenticates and authorizes every request.**
   - Users sign in with the organization's Microsoft Entra ID.
   - The backend accepts only its own tenant, the configured Azure DevOps organizations, and the configured users, groups or roles. It verifies organization membership with Azure DevOps at sign-in.
   - A rogue URL can't impersonate your backend's sign-in. Restricting user consent to unverified applications in Entra ID blocks look-alike sign-ins.
3. **Trusted hostname policy, stored URL validation and HTTPS** are rechecked on every load.
4. **Tamper detection.** A malformed or tampered configuration document is rejected.

Recommended: restrict user consent in Entra ID, use a trusted hostname policy, and review the change history regularly.

## Pull-request documentation checks

Shown under **Amy logo > Admin settings > Pull-request documentation checks** when the backend offers them (see [DEPLOY.md](DEPLOY.md#optional-pull-request-documentation-checks)). Changing them requires membership in Project Collection Administrators (when the backend has its own Azure DevOps access) or the backend's administrator app role (`Amy.Admin` by default); everyone else sees them read-only.

- **Repositories.** Add repositories and choose per repository whether completed pull requests are checked **automatically** or only on demand.
- **Wiki scope.** The Wikis, optionally limited to a path, that Amy reads and proposes changes for.
- **New pages are proposed under.** The parent path for release notes and upgrade and migration instructions.
- **Reviewers to notify.** Their e-mail addresses. Amy resolves them to Azure DevOps users and @mentions them on the completed pull request when it has prepared proposals.

Proposals and the pull-request metadata behind them are visible to everyone in the organization who uses Amy. Enable only repositories and Wikis whose documentation impact may be shared that widely.

## Disable AI

- **For the organization:** clear **Enable AI features** and save.
- **Backend side (all organizations it serves):** set `AI_DISABLED=true`, or remove the organization from `ALLOWED_ORGANIZATIONS`.

## Administrator responsibilities

- Operate or select the backend. Its AI provider, subscription and costs belong to your organization.
- Set usage limits, allowed organizations and allowed users or groups on the backend (see [DEPLOY.md](DEPLOY.md)).
- Confirm that your AI provider's data-processing terms cover the content listed in [DATA-HANDLING.md](DATA-HANDLING.md).
- Rotate backend secrets (procedure in [DEPLOY.md](DEPLOY.md)).
