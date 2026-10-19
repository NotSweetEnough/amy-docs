# Permissions

## Extension scopes

| Scope | Why it is needed | Used by |
| --- | --- | --- |
| `vso.wiki_write` | Read Wiki pages (including the persona pages of an earlier version, for a one-time import); save edits (version-checked) and create pages (create-only) **after the user confirms** | Hub, documentation assistant, Ask the Wiki |
| `vso.work_write` | Read and query work items, types, area and iteration paths; create or update work items (also a connecting Epic and its links), create iterations and add them to a team's sprints, each **after the user confirms** | Hub, Ask about Work Items, Amy on work items, Create Sprint Schedule |
| `vso.project` | List projects and teams, read the process template | Hub |
| `vso.code` | **Read-only** search of merged and active pull requests, pull-request metadata (title, description, branches, file paths, commits, linked work items), the repository list, and repository files for architecture documentation, debates and pull-request reviews. Reviews post nothing to the pull request | Pull-request docs, organization settings, Generate Project Documentation (architecture), Debate Architecture, Review a Pull Request |
| `vso.build` | **Read-only** classic (designer) build pipeline definitions, for the deployment view | Generate Project Documentation (architecture) |
| `vso.release` | **Read-only** classic release pipeline definitions and their stages, for the deployment view | Generate Project Documentation (architecture) |

Not requested: code write, build or release execution, identity management, security management, organization administration. Pipelines are only read, never created or changed.

The extension never asks for a personal access token. All Azure DevOps calls run with the signed-in user's own permissions, so users can only read or change what they could anyway. Extension data (the backend configuration and project templates) is the extension's own storage and needs no scope.

The hub is a top-level group in the project sidebar (`ms.vss-web.project-hub-groups-collection`); the work-item form group is named "Amy". Neither needs a scope.

## Organization-level actions

| Action | Who | Notes |
| --- | --- | --- |
| Install the extension | Organization or project collection administrators (Marketplace) | Re-approval is required when scopes change |
| Configure the backend | Project Collection Administrators (admin page) | See the limitation in [ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md) |
| Use AI features | Users allowed by the organization's backend (tenant, organization, users, groups, app role) | Enforced by the backend |
| Write Wiki pages or work items | Users with the corresponding Azure DevOps permission | Always after explicit confirmation |

Normal features need no administration permissions.

## Backend permissions (reference deployment)

- **Backend app identity:** Key Vault Secrets User, read-only, on the needed secrets.
  - With `STATE_STORE=blob` it also has Storage Blob Data Contributor on its state container (pull-request proposals, MCP proposals, architecture jobs, personas).
  - In Azure DevOps (pull-request checks, `ADO_AUTH_MODE`) it has Basic access, Read on the enabled repositories and Wikis, and Contribute to pull requests. It also reads the signed-in user's group memberships to recognize Project Collection Administrators as backend administrators; organization members can read memberships by default.
- **No separate sign-in (`AUTH_MODE=azure-devops`):** no backend identity and no app registration. When a session starts, the backend receives the user's Azure DevOps access token, with the extension's permissions above, and uses it once to ask Azure DevOps who the user is; it is neither stored nor logged.
- **Microsoft sign-in (`AUTH_MODE=entra`) and MCP:** no backend identity. Delegated, read-only Azure DevOps permissions on the backend's Entra app registration: `vso.profile` (membership check at sign-in), `vso.wiki`, `vso.work` and `vso.code` (MCP tools acting as the signed-in user). Not `user_impersonation`: the backend never writes to Azure DevOps on a user's behalf.
- **Secret administrators:** Key Vault Secrets Officer.
- **Deployers:** resource-group Contributor.

No identity gets Owner or Key Vault Administrator for runtime use.
