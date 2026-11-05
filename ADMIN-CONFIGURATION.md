# Organization administrator configuration

Amy does not include a hosted AI service. After installing the extension, an Azure DevOps organization administrator connects it to a compatible backend that the organization operates (see [DEPLOY.md](DEPLOY.md)). Until then, users see "AI features are not configured for this Azure DevOps organization" and the non-AI features keep working.

## Configure

1. In Azure DevOps, open **Organization settings > Amy AI**, tab **Backend settings** (or select **Admin settings** in Amy's logo menu). Azure DevOps decides who can open Organization settings; Amy adds no check of its own.
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
| Backend session token | The browser tab's `sessionStorage` (memory only where the browser blocks it) | Until it expires (`SESSION_TTL_MINUTES`), the tab closes or the backend rejects it |
| Project templates | Azure DevOps extension data, collection scope, one document per project | That project; changes need review and approval in **Project Templates** |
| Pull-request check settings (repositories, Wiki scope, reviewers) | The organization's backend | This organization only |

Nothing else is stored in the browser: no `localStorage` and no cookies.

## Who can change the configuration: platform limitation

Azure DevOps extension data offers no access control: any user who can run the extension can technically write collection-scoped extension data with their own credentials. The page lives in Organization settings, which Azure DevOps shows according to its own permissions, but that does not restrict the stored document either.

Amy protects users with checks that do not depend on who wrote the configuration:

1. **User confirmation.** Before any content is sent, each user sees the backend's name and hostname, plus who configured it and when. They must confirm it the first time and after every change.
2. **The backend authenticates and authorizes every request.**
   - Without a separate sign-in (`AUTH_MODE=azure-devops`, the default), Azure DevOps itself confirms each user in the organization when a session starts; the backend accepts only the configured Azure DevOps organizations.
   - With a Microsoft sign-in (`AUTH_MODE=entra`), users sign in with the organization's Microsoft Entra ID, and the backend accepts only its own tenant, the configured Azure DevOps organizations, and the configured users, groups or roles. It verifies organization membership with Azure DevOps at sign-in.
   - A rogue URL can't impersonate your backend's sign-in. Restricting user consent to unverified applications in Entra ID blocks look-alike sign-ins.
3. **Trusted hostname policy, stored URL validation and HTTPS** are rechecked on every load.
4. **Tamper detection.** A malformed or tampered configuration document is rejected.

Recommended: restrict user consent in Entra ID, use a trusted hostname policy, and review the change history regularly.

## Pull-request documentation checks

Shown under **Organization settings > Amy AI > Pull-request documentation checks** when the connected backend offers them; its operator switches them on (see [DEPLOY.md](DEPLOY.md#optional-pull-request-documentation-checks)). Changing them requires membership in Project Collection Administrators (when the backend has its own Azure DevOps access) or the backend's administrator app role (`Amy.Admin` by default); everyone else sees them read-only.

- **Repositories.** Add repositories and choose per repository whether completed pull requests are checked **automatically** or only on demand.
- **Wiki scope.** The Wikis, optionally limited to a path, that Amy reads and proposes changes for.
- **New pages are proposed under.** The parent path for release notes and upgrade and migration instructions.
- **Reviewers to notify.** Their e-mail addresses. Amy resolves them to Azure DevOps users and @mentions them on the completed pull request when it has prepared proposals.

Proposals and the pull-request metadata behind them are visible to everyone in the organization who uses Amy. Enable only repositories and Wikis whose documentation impact may be shared that widely.

## Sprint documentation

Shown under **Organization settings > Amy AI > Sprint documentation** when the backend has its own Azure DevOps access. Who may change it is decided as for the pull-request documentation checks above. Nothing runs until you add a team.

- **Add a team.** Choose the project and team, the Wiki and the folder for the pages, the documents (sprint documentation and release notes are preselected; checklists, testing summary and the other types are available), and optionally the days to wait after the sprint.
- **Automatic.** When a team's sprint is over, Amy reads its completed work items and prepares each document by itself. No one has to start it. Saving checks that the sprints can be read, so a missing permission shows at once.
- **Approval.** The documents wait under **Documentation > Review Pull Request Documentation** (origin "End of sprint"). A page is created when someone approves it, with their own Wiki permissions. Nobody is notified, so someone needs to look there at the end of the sprint.
- **Generate last sprint.** Prepares the documents of the team's last ended sprint now: for trying it out, and for a sprint that ended before the team was added, which is otherwise left out.
- **Latest documents.** What happened to each one, including why one was not generated: no completed work items, or a page with that name already exists.

Azure DevOps keeps no sprint goal, so the documents mark it TODO. A sprint counts as over when its last day has ended in UTC; set the days to wait if your team closes work later than that. Each team needs its own Wiki folder, because sprints of different teams can have the same names.

## Search and diagnosis

Shown under **Organization settings > Amy AI > Search and diagnosis** when the backend has Azure AI Search ([DEPLOY.md](DEPLOY.md#optional-ask-with-search-and-diagnose-a-ticket)). Changing it requires the same administrator rights as the pull-request checks.

- **Index status.** Ready, the passages indexed, the last run's counts and problems, and **Rebuild the index**.
- **Projects to index** and **Repositories left out.** Everything else of these projects is indexed: Wikis, default branches, work items.
- **Boards and their keywords.** A board is an area path, the same list as Create Sprint Schedule's Board field; it covers the areas below it. With several indexed projects, **Board's project** chooses whose boards and repositories are listed (default: the project Amy is open in). Questions and tickets that name its keywords are searched in its area, repositories and log services first.
- **Logs and traces.** Grafana's data sources Amy may read, with a **Test** that shows a masked sample and the service names found; a failed test names Grafana's reason. Tempo is read through Grafana's data source proxy (Tempo's own search and trace API, as Explore does). It follows trace IDs; for other request IDs, set **Attribute that holds the request ID** (for example `span.http.request_id`).
- **Diagnose a Ticket.** Where new Bugs and User Stories go (project, default area from that project's areas, iteration) and the L2 support group named in handover notes. When the board a diagnosis points to is in another project, the work item gets the default area instead.

## Code

Shown to organization administrators under **Organization settings > Amy AI > Code** when the backend offers Code with Amy ([DEPLOY.md](DEPLOY.md#optional-code-with-amy)).

- **Who has it, besides organization administrators.** Nobody by default: only Project Collection Administrators (and the backend's admin application role) see the card. Teams and groups added here see it too, nested groups included, on the Code tab; the backend refuses everyone else on every route. This is the feature's own rule: Access by tab neither opens nor closes it, so a team allowed here gets the Code tab with this card even where the tab is limited to others.
- **Projects and repositories.** What Amy may change. Nobody can run it on anything else, and a person also needs Contribute on the repository in Azure DevOps.
- **Reviewers of the pull requests.** Teams and groups added to each pull request, required or optional. Without any, the branch policies decide; who asked for the change is always added as an optional reviewer.
- **Target branches.** Pull requests go to the first of these that exists in a repository, otherwise to its default branch.
- **Protected folders and files.** Changed only when the approved plan or the person names them. Pipelines and agents' instruction files always are.
- **Models.** For the plan and its check, for code and tests, and for review, at high effort. Where none is set, the person's own choice or the persona's, and otherwise the largest the backend offers. A smaller model for code and tests costs about half, though a failed round can cost more than that saves.
- **Fix rounds.** How often Amy fixes what her reviewer found (2 by default) and what failed in the checks (3 by default) before a person decides.

## Repository documentation

Shown under **Organization settings > Amy AI > Repository documentation** when the backend offers Repository Documentation ([DEPLOY.md](DEPLOY.md#optional-repository-documentation)). Everyone sees the settings; changing them requires the same administrator rights as the pull-request checks. Until they are saved once, no repository is enabled.

- **Projects** whose repositories may get documentation, and per project the **repositories** to leave out. Users choose among the enabled repositories they can see.
- **Reviewers of the pull requests.** Search Azure DevOps teams and groups and add up to 10; each pull request gets them, as **Required reviewers** unless you clear that option. The person who started the run is added as an optional reviewer. At least one team or group is needed: the settings can't be saved without one once projects are enabled, a run doesn't start without one, and when none of them can be found any more, a repository's pull request isn't opened (its files can be downloaded, and Retry uses the settings as they are then).
- **Documentation folder**, `docs` by default: where the documentation set goes in each repository. Files people wrote there keep their text; Amy's part of a file sits between markers.
- **Also correct what the code contradicts in README files, CLAUDE.md and the folder's documents** (on by default): a second commit in the same pull request.
- **Open as drafts:** reviewers are notified when someone publishes the pull request.
- **Security assessment:** **Full detail** (the default), **Summary only** (findings without code or analysis) or **Left out**. The documentation goes into the repository, so everyone who can read the repository reads the findings: choose **Summary only** where that audience is wider than the people who should see how to exploit them.
- **Keeping the documentation up to date.** **Update the documentation when a pull request merges into the default branch** (on by default) applies to repositories that already have documentation, on the default branch or in Amy's open pull request. It needs an Azure DevOps service hook: Web Hooks, event **Pull request updated** (and optionally **Pull request merge attempted**), on the project or the organization, to `POST /api/hooks/pull-request` on the backend with the header `x-amy-hook-secret` set to `PR_DOCS_HOOK_SECRET`. The hook of the pull-request checks is the same one; a backend without those checks needs only the secret. The rules choose the model for a change: **Up to** so many changed code files (tests aren't counted), **More files**, **Paths** that hold certain words whatever the size, and the **Effort**. A model not chosen is the default shown. A run someone starts uses these rules too, for the code changes it takes up.

- **Dev documents** (off by default). Two documents in each enabled repository that has a dev branch, by pull request into that branch: `main-dev-differences.md` and `unreleased.md`. **Folder** (`dev-docs` by default, outside the documentation folder), **Dev branches** (the first a repository has; a repository with none is left out), **Repositories with another dev branch**, **Every** so many days (14 by default) **Starting on** a date (a day or two before a release is the useful moment; empty: one interval from now; a past day doesn't count, today does), the **Model** (the smallest the backend offers unless you choose) and their own **Reviewers**. With no team or group, nobody is asked to review and each pull request completes by itself once the dev branch's policies pass; a policy that wants a person still waits for one. This applies to the dev documents only: the documentation always asks a team or group. Check that the pipelines of the dev branch ignore this folder, like the documentation folder.

Without the hook nothing updates by itself; a merge that was missed is taken up by the next one, or by a run someone starts, because an update starts from the commit the documents follow. Updates are shown as started by **Amy (automatic)**, and only administrators can stop, discard or retry them. They run on what the backend's identity can read, not a person's rights, like the rest: **enable only repositories that everyone who uses Amy may read.**

The backend reads the repositories with its own identity. It checks that the user who starts a run may read the chosen repositories, and the checks of findings against connected services read only enabled repositories that user may read; quotes from them can appear in the documentation of the repository being documented. Everyone with the Code tab sees every run: repository names, pull-request links and counts of findings by severity (limit that with [access by tab](#access-by-tab)). Only who started a run and administrators can stop, discard or retry it or download its files. Enable only repositories that everyone who uses Amy may read. The backend's identity needs Read, Contribute, Create branch and Contribute to pull requests on them; without them a repository ends as **Needs permission**, and its files can be downloaded instead.

## Access by tab

Shown under **Organization settings > Amy AI > Access by tab** when the backend has its own Azure DevOps access (`ADO_AUTH_MODE`) and, in production, `STATE_STORE=blob`. Only administrators (Project Collection Administrators or the backend's admin role) see and change it.

- **One rule per tab**, for the whole organization: Wiki, Work Items, Documentation, Code, Support, Requirements, Others, and **Amy on work items** (the panel on work item forms). A tab without a rule is open to everyone.
- **Add to <tab>** finds Azure DevOps teams (any part of "[Project]\Team") and groups (the beginning of their name). Once a tab has a team or group, only their members, also through nested groups, and administrators can use it. Administrators always have every tab.
- **Membership is Azure DevOps'.** Add and remove people in the team as usual; Amy applies a change within about 5 minutes, and saved rules within a minute. Entra groups count when they were added to Azure DevOps.
- **The backend enforces it.** Amy hides the tabs a user doesn't have, and the backend refuses their requests. Ask Amy and Ask about Work Items share the chat: it's there with either tab, and searches only the sources of the user's tabs (Repos needs the Code tab). Project Templates and Create Sprint Schedule (Others) run in the browser with the user's own Azure DevOps permissions: limiting Others hides them.
- **When Azure DevOps can't be reached**, limited tabs stay closed until it can; open tabs keep working.

## Disable AI

- **For the organization:** clear **Enable AI features** and save.
- **Backend side (all organizations it serves):** set `AI_DISABLED=true`, or remove the organization from `ALLOWED_ORGANIZATIONS`.

## Administrator responsibilities

- Operate or select the backend. Its AI provider, subscription and costs belong to your organization.
- Set usage limits, allowed organizations and allowed users or groups on the backend (see [DEPLOY.md](DEPLOY.md)).
- Confirm that your AI provider's data-processing terms cover the content listed in [DATA-HANDLING.md](DATA-HANDLING.md).
- Rotate backend secrets (procedure in [DEPLOY.md](DEPLOY.md)).
