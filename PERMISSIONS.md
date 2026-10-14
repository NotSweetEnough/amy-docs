# Permissions

## Extension scopes

| Scope | Why it is needed | Used by |
| --- | --- | --- |
| `vso.wiki_write` | Read Wiki pages; save edits (version-checked) and create pages (create-only) **after the user confirms** | Hub, documentation assistant, Ask the Wiki |
| `vso.work_write` | Read work items, types, area and iteration paths; create or update work items, create iterations and add them to a team's sprints, each **after the user confirms** | Hub, bulk operations, Amy on work items, Create Sprint Schedule |
| `vso.project` | List projects and teams, read the process template | Hub |
| `vso.code` | **Read-only** search of merged pull requests, pull-request metadata (title, description, branches, file paths, commits) and the repository list | Pull-request docs, organization settings |
| `vso.graph` | **Read-only** group memberships of the current user, to show the admin page's controls only to Project Collection Administrators | Admin page |

Not requested: code write, build, release, identity management, security management, organization administration.

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
  - With pull-request checks it also has Storage Blob Data Contributor on its state container.
  - In Azure DevOps it has Basic access, Read on the enabled repositories and Wikis, and Contribute to pull requests.
- **MCP:** no backend identity. Delegated Azure DevOps `user_impersonation` on behalf of the signed-in user.
- **Secret administrators:** Key Vault Secrets Officer.
- **Deployers:** resource-group Contributor.

No identity gets Owner or Key Vault Administrator for runtime use.
