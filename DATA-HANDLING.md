# Data handling

## Who receives data

Amy runs in the browser inside Azure DevOps. AI features send content **only to the backend your organization administrator configured**. That backend is operated by your organization or a provider it selected, and it forwards content to the AI provider it is configured with.

The Marketplace publisher operates **no backend, no AI subscription and no data store** for Amy, and receives no Wiki, work-item, pull-request or usage data. The extension does not include a shared or default remote backend.

## What is sent, and when

Content is sent only when a user triggers the corresponding action:

| Action | Content sent to the configured backend |
| --- | --- |
| Generate backlog / regenerate item | Markdown of the selected Wiki page, the instruction, page title and path, process work-item types, area and iteration path names |
| Test Cases | Story title, description, acceptance criteria |
| Documentation assistant | Markdown of the loaded Wiki page and the command or instruction |
| Ask the Wiki | The question and the most relevant passages (page title, path, section, text) from the chosen scope. In scopes over 40 pages, the question (with the earlier turns, if session memory is on) first goes to the backend once more for search terms, and those terms and the question's keywords go to Azure DevOps Wiki Search, inside your organization and with the user's own permissions, to choose the pages. With **Session memory** on (1.1, default), also the chat's last six earlier questions and answers (answers shortened); they are kept in the browser only until the user leaves the chat. With **Web search** on (1.1), the backend may send the question, and only the question, to the web search service your organization configured |
| Amy on work items (more actions) | Title, description, acceptance criteria, repro steps and comments of the selected work items |
| Release documentation | ID, type, title, state, description excerpt and link of the selected work items; the sprint goal and notes the user entered |
| Pull-request docs (1.0 backends) | PR title, description, branch names, changed file paths, commit messages (no code diff) and relevant Wiki passages |
| Create a Wiki Page | The prompt, the chosen template's section names, and optionally the selected page's Markdown and the selected work items' text |
| Summarize Wiki Content | Passages of the pages in the chosen scope (page title, path, section, text) and the optional focus |
| Create Work Items | The title and short description you typed |
| Ask about Work Items (1.1) | The question and the user's local date; then, for up to 300 work items the user can read that match the filter: ID, type, title, state, dates, area and iteration path, tags, parent title, and the description and acceptance criteria shortened to 600 characters each. With Both, also Wiki passages as for Ask the Wiki |
| Generate Project Documentation (architecture) (1.1, off unless the backend enables it) | **Source code**: the selected files of the chosen repositories, a share of each service's most informative files or all of them where the user chose so (at most the backend's upload budget, 25 million characters by default), with values that look like keys, passwords or connection strings masked; secrets files are never read. Also facts found by patterns in all other code and configuration files of the documented services (endpoint paths, trigger, queue, topic and schedule names, outgoing addresses, connection string names, setting and environment variable names, ports and base images, each with its file and line; masked like uploads, without the files' content), runtime data the user adds (caller and target names, dependency types and call counts exported from Application Insights; names and types of deployed apps exported from Azure Resource Graph), the paths of the other files, classic pipeline definitions (masked), and the pages of the chosen Wikis (this project's Wiki and, if the user chooses them, Wikis of other projects the user can read; all their pages or the pages the user picks) except the documentation trees. The Wiki pages the documentation cites are copied into it, in this project's Wiki, once the user approves the pages; copies of other projects' pages can then be read by everyone who can read this project's Wiki. Accepted for Amy on 2026-10-15; each organization decides by enabling the backend setting |
| Generate Project Documentation (project management) (1.1, off unless the backend enables it) | The pages (path, title, Markdown) of the chosen Wikis: this project's Wiki and, if the user chooses them, Wikis of other projects the user can read (their pages under `/<project>/<wiki>`, their Wiki links as plain text), except the architecture documentation, the management documentation and old `/Amy` persona pages; no source code. The new tree is saved only in this project's Wiki |
| Debate Architecture (1.1) | The question, the conversation's earlier turns (shortened), passages of the documentation tree, and files the model asked for (masked), read with the user's permissions |
| Review a Pull Request (1.1) | **Source code**: both versions of each changed file of the chosen pull request (within a size budget), with values that look like keys, passwords or connection strings masked; lock files, secrets files, binaries and generated files are never read. Also the pull request's title, description, branches and commit messages, the paths of skipped files, the review focus and instruction, the linked work items (title, description, acceptance criteria), and passages of the documentation tree. Read with the user's permissions; nothing is posted to the pull request |
| Check Repository Documentation (1.1, with architecture documentation) | **Source code**: of the chosen services, their documents (READMEs, docs, OpenAPI and Swagger definitions), configuration, manifests and container files, and as much of their other code as the backend's upload budget allows, with values that look like keys, passwords or connection strings masked; secrets files are never read. Also the facts found by patterns in all their code, as for the architecture documentation. The backend keeps the files until the check ends; per service it keeps the last result (to reuse it while nothing changed) and the checked documents' paths and contradicted statements (for the architecture documentation). The report is saved in this project's Wiki only after the user's review; dismissals are kept as project data |
| Update the Wiki (1.1) | The instruction, and the pages of the active Wiki in the chosen scope (all pages or the chosen ones, except `/Amy`), read with the user's permissions. The backend keeps the pages until the edits are proposed, and the result until the user saves or discards it; the AI receives the passages that match the change (or every page, if the user chooses so), the affected pages' mentions, the user's answers, and each chosen page in full for its edits. The extension saves the approved edits with the user's permissions |
| Personas (1.1) | Nothing from the browser with AI requests: the backend applies the organization's personas itself. Administrators send the persona texts and defaults when they save them. The one-time import reads the project's earlier `/Amy` Wiki pages with the administrator's permissions |
| Field controls on work items | The field's current text (Improve only; never for Regenerate), the title and the other supported text fields, the template outline |
| Pull-request documentation checks | Nothing from the browser except the time frame and scope of on-demand reviews. The backend reads PR metadata (no code diff) and the configured Wiki pages itself |
| MCP (AI clients) | The tool arguments the AI client sends; the backend reads the named Wiki pages and work items with the user's own permissions |

Every request also carries a correlation ID, the Azure DevOps organization name and, as a label, the current project name. Without a separate sign-in (`AUTH_MODE=azure-devops`), when a session starts the extension sends the user's Azure DevOps access token once: the backend asks Azure DevOps who the user is in the organization (`connectionData`), issues its own session and discards the token; it is neither stored nor logged. The token carries the extension's permissions (see [PERMISSIONS.md](PERMISSIONS.md)) and expires on its own. Backends offering `ai-settings` also receive the user's AI settings (model, thinking, response length, web search); the extension keeps them in memory for the conversation only.

## What the extension stores

- **Organization configuration:** backend URL, display name, flags, API version, trusted hostnames and change history, in Azure DevOps extension data.
- **Per user:** whether the user has confirmed a backend hostname, in Azure DevOps extension data.
- **Backend session token:** in the browser tab's `sessionStorage` until it expires, so Amy's pages in the same tab share one sign-in; removed when the tab closes or the backend rejects it.
- **Project templates:** template names and section names per project, in Azure DevOps extension data.
- **Architecture documentation choices:** per project, the team's changes to the Services table (names, kinds, merges, what to read) and the Code-vs-Wiki differences dismissed (the difference's kind, Wiki page and quote or page and code file, the reason, who dismissed it and when), in Azure DevOps extension data, readable by everyone who uses Amy in the organization. Generate Project Documentation also sends the team's **Architecture Facts** page, the dismissed differences and the names and last activity dates of repositories left out as possibly retired to the backend with the job.

Nothing else is stored in browser storage.

## User confirmation

- Users confirm the backend before first use and after every change.
- Wiki and work-item changes require explicit confirmation: a creation summary, per-change review and an extra confirmation above 10 items.

## Reference backend behaviour

The reference backend (`backend/`):

- **Content:** does not store request content. Logs contain metadata only: correlation ID, operation, organization, pseudonymous user reference, latency, outcome, token counts.
- **Credentials:** redacts credentials.
- **Per-organization records:** keeps usage counters and audit records per organization, in memory.
- **Usage counts (when offered):** how many times each quick action ran per day, per organization, under `orgs/<organization>/usage/<month>`. Counts only: no content, no user names or references. Shown to administrators under Organization settings > Amy AI > Monitoring.
- **Personas (when offered):** the editor's display name with each entry's last change, plus the organization's identity and per-quick-action personas (text, default model, effort, lock), the last editor and a history of the last 20 changes (time, editor name, changed personas), in the state store under `orgs/<organization>/personas`. Only administrators can read or change them: Project Collection Administrators (the backend looks up the signed-in user's Azure DevOps groups with its own access and keeps the answer in memory for 5 minutes) or people with the backend's admin role. With `STATE_STORE=memory` they are lost on restart.
- **Architecture documentation jobs (only when enabled):** in the organization's own storage account, visible to the user who started them. The uploaded code and intermediate results are kept until the job succeeds (for retries), then deleted; the result is deleted when the user creates the pages or discards it. One result per user and project: a new run replaces an unused one. No time limit applies.
- **Stored proposals (only when pull-request checks or MCP are enabled):** in the organization's own storage account, keyed by organization. It keeps:
  - pending and decided Wiki proposals (page path, proposed changes or new page content, reasons, source pull requests);
  - backlog proposals from AI clients;
  - pull-request review records (status, impact summary);
  - service-hook delivery IDs;
  - the check settings.

  Proposals from AI clients are visible to their requester only. Pull-request proposals are visible to the organization's Amy users. Blob versioning and a 14-day soft delete apply; delete blobs under `orgs/<organization>/` to remove an organization's data.

