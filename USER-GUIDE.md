# User guide

Open a project in Azure DevOps and select **Amy** in the project sidebar, next to Boards, Repos, Pipelines, Test Plans and Artifacts. The header shows the current project and whether AI is available.

**Before AI features work:**

1. **Backend configured.** An organization administrator must have connected a backend that your organization operates ([ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md)). Otherwise you see "AI features are not configured for this Azure DevOps organization". Loading and editing Wiki pages and creating work items still work.
2. **Confirm the backend.** The first time, and after any change, confirm the backend your content is sent to. Amy shows its name and hostname.
3. **Sign in.** Select **Sign in**. A pop-up uses your organization's Microsoft account; allow pop-ups for Azure DevOps. If you are already signed in to Microsoft, the pop-up closes by itself. The sign-in lasts for this browser tab, also when you open Amy again, until it expires (an hour by default) or you close the tab.

Quick actions that your organization's backend does not offer say so on their box.

## The Amy hub

From top to bottom: the header with Amy's logo (AI settings) and the connection status (select it for details), five tabs, the **Wiki scope** filter (on the Wiki tab only), the quick action cards, and the workspace with the conversation.

| Tab | Quick actions |
| --- | --- |
| **Wiki** | Ask Amy, Create a Wiki Page, Edit a Wiki Page |
| **Work Items** | Ask about Work Items, Create Work Items, Generate Backlog Hierarchy |
| **Documentation** | Review Pull Request Documentation, Generate Release or Sprint Documentation, Generate Project Documentation, Debate Architecture |
| **Code** | Review a Pull Request |
| **Others** | Project Templates, Create Sprint Schedule |

Amy opens with **Ask Amy**. Selecting a tab shows its cards; selecting a card starts its task, and the cards collapse into a bar with the task's name and **Change action**. Drafts are kept when you switch tasks and come back.

**Wiki scope.** Questions and summaries use the active Wiki of the project (switch it next to the filter if the project has several):

- **Entire Wiki:** all its pages.
- **Selected pages:** search and tick one or more pages.

The **Wiki Scope Preview** beside the conversation shows "Entire wiki selected", the selected page, or the selected pages to open one by one. **Hide** collapses it.

**Chat.** The composer, **Ask, summarize Wiki or describe what you need**, belongs to Ask or Summarize Wiki and is shown only while that task is open. It does three things:

- A question with **Send** is answered from the Wiki scope, in the conversation, with links to the supporting pages.
- **Summarize** summarizes the Wiki scope in the conversation. Typed text, if any, is the focus. **Publish as Wiki page…** shows the page for review; it is created only when you approve.
- Other requests start the matching task with your text filled in, for example "Create a user story for CSV export", "Find duplicates of the tracking feature" or "Check the documentation quality of this page". Amy tells you which task it started.

**Wiki Only Answer Mode**, beneath the composer's buttons, is on by default: answers use only the selected Wiki content and say so when it is not enough. Switched off, general knowledge is shown separately from what the Wiki supports. Amy never saves anything from a chat message alone.

**AI settings.** Select Amy's logo, left of the title, to open **AI Settings** beneath it. Changes apply right away to your next requests; there is no Save button, and requests already running keep the settings they started with. The settings last for this conversation (until Amy is reloaded) and close with Escape, a click elsewhere or the logo again.

| Setting | What it does |
| --- | --- |
| **Model** | The models your organization approved, marked Local (its own infrastructure) or Cloud. The default is the organization's configured model. |
| **Thinking** | On: the model reasons before answering (slower, often more thorough). Available only for models that can switch it; otherwise it is disabled with an explanation. A new model starts with its own default. |
| **Session memory** | On by default. Amy remembers the chat's earlier questions, answers and summaries (the last six, answers shortened), so follow-ups such as "and who approves it?" work; a follow-up is searched in the Wiki together with the question before it. Leaving the chat (another task or tab) ends the session: the conversation and Amy's memory of it are cleared. Off: each question is answered on its own. Earlier answers are context only; answers still cite the Wiki. |
| **Response length** | Short (brief and direct), Standard (balanced, default) or Large (detailed). It changes the wording only: required fields, items, citations and structured results are never cut. |
| **Web search** | Off by default, and available only when your organization's backend has a search integration. On: a question searches the web when the Wiki does not answer it, and always when you ask Amy to search the web. Only your question is sent to the search service. Web findings appear separately, "From the web (not from the Wiki)", with links to their sources. Turning web search on turns **Wiki Only Answer Mode** off; turning Wiki Only Answer Mode on turns web search off. |

The settings never change your permissions, the Wiki scope or the approval steps; the backend checks them on every request.

**Model per quick action.** The **Model** list starts at **Set by your organization**: each quick action then uses the model and effort your organization's administrators set for it. Choose a model (and **Thinking**) to use it in every quick action instead. A quick action your organization **locked** always uses its own model; the menu shows 🔒 and which model applies.

**Admin settings.** The last entry of the menu, **Admin settings**, opens **Organization settings > Amy AI** in a new browser tab, with tabs for **Backend settings** (the organization's AI backend), **Personas** (when the backend offers them) and **Pull-request documentation checks** (when the backend has them switched off, the tab lists what its operator has to configure). Azure DevOps decides who can open Organization settings. Administrators (Project Collection Administrators, or people your backend operator gave the admin role) also see **Monitoring**: a table of how often each quick action ran in the last 7, 30 or 90 days. One question, generation, review or documentation job counts once; only counts are kept, no content and no names.

**Personas (administrators).** Under **Organization settings > Amy AI > Personas**, for the whole organization:
- the **identity** (who Amy is: voice and house rules; `{project}` is replaced by the current project's name), and
- one persona per quick action (Ask and Summarize, Create a Wiki Page, Edit and Review Wiki Pages, Create Work Items and Backlogs, Work Item Improvements, Test Cases, Pull Request Documentation, Release and Sprint Documentation, Architecture Documentation, Architecture Debate, Code Review, Project Management Documentation), each with its text, a **Default model**, a **Default effort** (Model default, Off, Low, Medium, High; only what the model supports) and **Lock** (users can't choose another model or thinking for it).

Personas are kept in your organization's backend, not in the Wiki: only administrators can see or change them (Project Collection Administrators, or people your backend operator gave the admin role), and other users' browsers never receive the texts. **Save** applies them to everyone's next request; **Restore the built-in text** brings back Amy's default for one persona, and each persona shows, like a Wiki page, who changed it last and when ("Built-in text, not changed yet" until someone does), and the change history lists who changed what. When nothing is saved yet and the project has persona pages from an earlier version (`/Amy` in the Wiki), Amy offers to **Import** them once; the Wiki pages are then no longer read and can be deleted. Personas never change the checks, citations, "not enough evidence" answers or the confirmation before anything is saved; your own Response length setting wins over a persona's length.

**Approval.** Everything that creates or changes content follows **Generate → Review → Approve → Save**:

- Existing content stays as it is until you approve its replacement.
- You can apply a proposal, request another version, or cancel.
- Where several proposals wait (pull-request documentation), you review each one and can approve selected ones together. Only approved changes are saved.

### Page-only mode

Open Amy for one page from a link, for example placed on the Wiki page itself:

```text
https://dev.azure.com/<organization>/<project>/_apps/hub/<publisher>.amy.amy-hub?amyWiki=<wiki ID>&amyPage=/Operations/Deployment%20Guide&amyScope=page
```

In page-only mode:

- Amy reads and edits only that page.
- Only Ask or Summarize Wiki and Edit a Wiki Page are offered (documentation quality review is a typed request).
- Other pages, Wikis or projects need the full Amy, where you select the broader scope explicitly (**Open full Amy**).

Azure DevOps offers extensions no place beside the Wiki page title, so this link takes the place of a button there.

## 1. Choose a Wiki page

Tasks that work on one page (Edit a Wiki Page, Generate Backlog Hierarchy, documentation quality review) start with a page search in the active Wiki; Generate Backlog Hierarchy, on the Work Items tab, has its own **Wiki** selector when the project has several. Type part of the name or path and pick the page; its content is shown right away. If one page is selected in the Wiki scope, it is preselected.

**Editing the page:**

1. Select **Edit page**, change the Markdown and select **Review changes**.
   - A side-by-side view shows **Current version | Proposed version**, with a summary of added sections, deleted sections and changed statements.
   - Reject any change you don't want, then select **Save page**. Use **Cancel editing** to discard everything.
2. **Documentation assistant:** choose a command and select **Run**.
   - **Analysis** (nothing is written): Review this page, Find missing sections, Identify ambiguous statements, Extract decisions, Extract dependencies and risks. Findings show severity, section, the quoted statement and a suggestion. **Copy findings** copies them as a Markdown table.
   - **Check against a template** compares the page's headings with one of ten versioned documentation templates: architecture overview, Azure service onboarding, AKS runbook, deployment, monitoring, support, incident troubleshooting, access management, API integration, maintenance. This check runs locally, without AI.
   - **Change commands:** Improve structure, Generate summary, Clarify ambiguous statements, Custom change request. Each one produces a change proposal.
     - Every proposed change shows the current and proposed text, the AI's rationale and any source references.
     - Accept or reject each change ("Accept all" / "Reject all" are available). Saving is possible only once every change is decided, and only accepted changes are written.
   - **Generate table of contents** inserts the Azure DevOps `[[_TOC_]]` tag after the title. **Add missing template sections** adds skeleton sections, each marked TODO. Both run locally and go through the same change review.
   - **Create onboarding version** / **Create operational runbook version** draft a new page, optionally using a template's sections. You edit the path and content, then create it as a new page; existing pages are never overwritten.
   - Results apply only to the page version they were made for.
3. If someone else changed the page after you loaded it, the save is rejected. Reload the page and apply your edits again.
4. Azure DevOps keeps every earlier version in the page history.

Code wikis (published from a repository) are read-only here.

## 2. Generate

1. Optionally enter an **Instruction**, for example:
   - "Create an Epic and User Stories from this Wiki page."
   - "Generate technical Tasks for implementing this design."
   - "Create only security-related User Stories."
2. Select the **work-item types** to generate. Types your project's process does not have are disabled, with an explanation. Some types are mapped instead, for example "User Story" is created as "Product Backlog Item" in Scrum or as "Issue" in Basic.
3. Select **Generate backlog**. This can take up to a minute.

## 3. Review and edit

- The backlog is shown as a tree (Epic > Feature > Story > Task). Use ▾/▸ to collapse or expand items.
- **Source-backed** items cite the Wiki. **AI-proposed** items do not, so review them carefully. AI assumptions, recommendations and warnings are listed above the tree.
- Click a title to open the editor. You can change the type, title, description, acceptance criteria, definition of done, notes, test scenarios, risks, dependencies, tags, estimate and priority. Then select **Save changes**.
- **Regenerate** asks the AI to rewrite just that item. **Add child** adds a manual item. **Remove** deletes the item and its children.
- **Test Cases** (on User Stories and Product Backlog Items) generates Test Cases from that story's acceptance criteria, using the test categories chosen in the generate form (default: happy path and negative).
  - Each Test Case shows its category and the criteria it covers (AC1, AC2, ... in criteria order).
  - Steps go into the Test Case steps field, and the Test Case is linked to the story with "Tested By".
- **Test Cases for an existing work item** (left column): enter a story ID, load it, choose categories and generate. The Test Cases replace the current preview, and the story is pre-selected as their parent.
- Use the checkboxes to choose what to create. Errors (for example a type that is unavailable in the target project) block creation. Warnings do not.

## 4. Create

1. Choose the target **Project**, optionally a **Team** (this pre-fills its default area and current iteration), the **Area Path** and the **Iteration Path**.
2. Optionally search for an existing **parent work item** by ID or title words. Selected items without a selected generated parent are linked under it. For example, generate Tasks and attach them to an existing User Story.
3. If the AI suggested Area or Iteration Paths (shown and editable in each item's editor), tick **Apply AI-suggested Area/Iteration Paths** to use them. Only suggestions that exist in the project are applied; other items use the paths selected above. The final summary shows how many items use a suggestion.
4. Keep **Check for possible duplicates** on. The check looks for:
   - similar titles
   - overlapping descriptions
   - similar items under the same existing parent
   - completed work (flagged)
   - similar Bugs
   - Wiki pages on the same topic

   All results are possible overlaps, never automatic decisions. Turn on **Dry run** to preview without creating anything.
5. Select **Review creation** and check the final summary. For each possible overlap, choose one of:
   - **Create anyway**
   - **Skip**
   - **Use existing #ID** (children are linked to it)
   - **Create and link to #ID** (adds a "Related" link)
   - **Merge into #ID** (appends the proposed content to the existing item instead of creating a new one; children are linked to it)
   - **Create and link to this page** (for Wiki pages)

   Use **Open** to inspect a candidate, and **Cancel creation** to stop without creating anything. New Wiki pages are also checked against existing page names; a similar name requires **Create anyway**.
6. If more than 10 items will be created, tick the confirmation box.
7. Select **Create work items**.

## 5. Results

The results table lists the type, ID (linked to Azure DevOps), title, parent and status (`created`, `existing`, `skipped`, `failed`, `dry-run`) of each item. If some items fail, the others are still created. Children of a failed item are skipped so that the hierarchy stays correct. Fix the cause and create the remaining items again; items that were already created are deselected automatically. **Copy summary** copies a tab-separated summary.

## Ask Amy

**Sources.** Choose **Wiki**, **Work items** or **Both** above the scope filter. **Ask about Work Items** (Work Items tab) opens the same chat with Work items selected. With work items among the sources:

- A **work-item filter** appears: **Time frame** (any time, last 30 days, this or last quarter, this year, custom range), **Date means** (completed, created or changed; completed by default), **Types** and **Area**. The time frame applies to work items only; with Both, the Wiki part is never filtered by date. When your question names a period ("in Q3", "last year"), Amy uses it and updates the filter.
- Overview questions ("a structured overview of everything we finished in Q3") get themes grouped by Epic with the items of each. Topic questions ("did we ever implement single sign-on with Entra?") search for the subject and related terms (shown in the answer) and get a verdict (implemented, partly, planned, not found), confidence and a table of the work items used, each linked and with the quoted evidence. Wiki sources are listed separately.
- Each answer starts with the filter it used; **Change filter** jumps to it. At most 300 items are read per question; the answer says when more matched.
- When related items share no Epic, **Connect them under a new Epic…** proposes one (title, area, description) and shows the link each item gets. Existing parents never change: Features without a parent become children of the Epic, other items get a Related link, items already under one of them need none. Nothing is created until you confirm.
- **Publish as Wiki page…** creates the answer as a new page, with a link to every work item, after you approve it.
- The checkbox reads **Sources Only Answer Mode**. Amy sees only work items you can read.

**Wiki.** Choose the Wiki scope, then type a question and select **Send**, or select **Summarize**. There is no separate question field and no page to load: the scope filter decides what is read (at most 40 pages). In a larger scope, a question reads the pages Azure DevOps Wiki Search finds for its keywords (the result says "found by Wiki search"); without the Search extension or a match, the first 40 pages are read and the result says the scope was cut.

Each answer lists its sources: page link, section heading, Wiki and the exact supporting passage, with a confidence level. If the pages don't contain the answer, you get **Insufficient evidence** instead of a guess. With **Wiki Only Answer Mode** off, general background appears separately as "General knowledge (not from the Wiki)". Retrieved Wiki content is treated as source material, never as instructions to Amy.

## Create a Wiki Page

1. Optionally choose a project template (for example Deployment guide).
2. Enter the page title and what the page should cover.
3. Optionally use the pages selected in the Wiki scope and work items (by ID) as sources.
4. Select **Draft page**. Amy first lists existing pages with a similar name; continue only if none of them should be edited instead.
5. The review shows the **destination Wiki** (the active Wiki; changeable here if the project has several), the title and location, the content and any similar pages. Edit what you need and select **Create Wiki page**. Existing pages are never overwritten. **Request another version** drafts it again.

## Create Work Items

Enter a **title** and a **short description**, choose the work-item types and select **Generate proposal**. The proposal opens in the same review as a generated backlog (sections 3 to 5). Possible duplicates are checked before creation. If your organization uses Amy's MCP server, proposals you requested from an AI client are listed here (**From your AI clients**); load one to review, edit and create its items.

To look for duplicates without creating anything, type for example "Find duplicates of the tracking feature". Amy lists existing work items, Bugs and Wiki pages that may cover the same work, with the reason for each match.

## Review Pull Request Documentation

**Search.** Choose the repository (when there are several), then either a **Start date** and **End date** (merge dates) or a **Pull Request ID**, and select **Search**. An ID is looked up in the selected repository; the dates are not used then. Searching never starts an analysis.

**Analyze.** The results table lists **Pull Request ID**, **Title** and **Documentation analysis**. Each row has its own action:

| State | Shown | Actions |
| --- | --- | --- |
| Not analyzed | **Analyze** | Analyzes only this pull request |
| Running | **Queued** or **Analyzing…** | — |
| Finished | **✓ Completed** | **View results**, **Reanalyze** |
| Failed | **✕ Failed** and the reason | **Retry** |

Results of the automatic checks are reused: a pull request already checked after completion shows **Completed** right away, and a running check is never started twice. **Completed** means the analysis finished, not that anything was approved or saved.

**Results.** **View results** shows the documentation impact and the pages Amy proposes to create or update, or that no updates are needed. **Review** opens the proposal in **Approval Requests**.

**Approval Requests.** The panel has two sub-tabs: **Approval Requests (N)** and **Find and Analyze** (search and analysis above). Approval Requests opens first when requests are waiting; the quick action card shows their number in a red bubble and the Documentation tab shows a dot. There is one request per Wiki page; a page changed by several pull requests is one request. Filter by **Repository** and **Origin** (pull requests or AI clients).

- **Update** proposals open with **Review changes**, side by side (**Current version | Proposed version**). Accept or reject individual changes, then select **Approve Changes**.
- **New page** proposals (release notes, upgrade and migration instructions) show a preview first, then **Approve Changes**.
- **Reject** removes a request without changing the Wiki. Select several requests to **Approve selected** or **Reject selected** together.
- Anyone who can edit the Wiki can approve or reject.

Before saving, Amy checks that the page has not changed since the proposal was prepared. If it has, Amy prepares the proposal again against the latest version and asks you to approve it again. When several pull requests affect the same page, they are combined in its pending proposal; every new revision needs a fresh approval.

**Automatic checks.** Completed pull requests in repositories an administrator enabled are also checked right after completion, whether or not Amy is open. Amy then @mentions the configured reviewers on the pull request; if nothing needs documenting, nobody is notified.

Without automatic checks on your organization's backend, **Analyze** runs in Amy: the results show the impact, the affected pages (with **Add PR link to page**, reviewed side by side) and draft release notes, upgrade and migration instructions.

Proposals you requested from an AI client through Amy's MCP server appear in the same list, labeled "Requested by you through an AI client". Only you see them.

## Generate Release or Sprint Documentation

1. Choose the documentation type: sprint documentation (default), release notes, deployment checklist, testing summary, known limitations, rollback plan, support handover, and more.
2. Choose the **Board** (team) and **Sprint**; the current sprint is preselected. Optionally enter the **Sprint goal** (Azure DevOps keeps none, so without it the document marks it TODO) and **Notes** with facts the work items do not contain, such as a release date, environments or known issues.
3. Select **Generate Sprint Documentation**. Amy lists the completed work items of the sprint and drafts the document.
4. Review and edit the page. Sprint documentation is named after the sprint (for example "Sprint 12"), other types "Sprint 12 – Release notes". Select **Create Wiki page** to approve its creation.

Selecting a release instead of a sprint is not available yet. To create the sprints themselves, use **Create Sprint Schedule**.

## Generate Project Documentation

Select **Documentation > Generate Project Documentation**. Your organization's backend must offer it; it is off unless an administrator enables it, because source code and Wiki content are sent to the backend and its AI provider. Two tabs, one job each (both can run and wait for review at the same time):

- **Create architecture documentation**: from the repositories and the Wiki, an arc42 tree under **`<project> Architecture Documentation`**, described below.
- **Create project management documentation**: the Wiki's content reorganized into a new tree under **`<project> Management Documentation`**, described after it.

The two trees don't repeat each other: the architecture documentation leaves out project management topics (roadmap, team, way of working, meetings) and links the management documentation instead, and the other way around for architecture topics (system structure, components, infrastructure, technical decisions).

### Create architecture documentation

1. **Start.** Choose the repositories (default branch). Under **Projects**, add other projects whose repositories belong to the same system, for example deployment or infrastructure projects; their repositories are then named "repository (project)" in the documentation, which is still created in the current project's Wiki. You see only repositories you can read. Amy shows what it reads: files found and selected (manifests, infrastructure as code, pipelines, configuration, API definitions, docs in the repositories, then source code across all folders), with **Show selected files** for the details and what is not uploaded (dependencies and build output, binaries, lock files, secrets files, generated files; tests are listed only). Classic build and release pipelines are read too. It compares with the entire Wiki and estimates time and tokens. Values that look like keys, passwords or connection strings are masked before upload.
2. **Main page.** The tree goes under **`<project> Architecture Documentation`** in the project Wiki. If a page with that name exists that Amy did not create, enter another name; Amy never changes pages it did not generate.
3. **Running.** The job runs on your organization's backend in four steps (analyze modules, build the system model, compare with the Wiki, write pages). You can close Amy meanwhile. When it is done, the card shows a small red bubble and the Documentation tab a dot; they clear when you open the result. A failed job shows the bubble too; **Retry from step N** continues where it stopped.
4. **Review.** The page tree follows arc42: the main page with a context diagram, 1 Introduction and Goals through 12 Glossary, a subpage per building block, runtime flow and crosscutting concept, **Code vs. Wiki Differences** (contradictions, parts missing in the Wiki, Wiki content not found in the code) and **About This Documentation**. Code references link to the analyzed commit; diagrams are Mermaid. Clear a box to leave a page out, then **Create N pages**. Creating or **Discard** deletes the stored result.
5. **Running it again** proposes changes to the existing tree only: changed pages side by side (accept or reject each change), new pages, and pages of parts that left the code (a note is added; pages are never deleted).

### Create project management documentation

1. **Start.** Amy shows how many Wiki pages it reorganizes: the whole Wiki except the architecture documentation, its own tree and old `/Amy` persona pages, up to 2,000 pages (a larger Wiki gets a warning: only the first 2,000 are included), with an estimate of the time and of the input and output tokens. If a page named `<project> Management Documentation` exists that Amy did not create, enter another name.
2. **Running.** Three steps on your organization's backend: **Understand the pages** (what each page holds), **Design the structure** (a page tree built from the content, closer to how software teams document a project; internal procedures and team-specific topics keep their own sections) and **Write pages**. For a Wiki of more than 150 pages, the structure is designed in two levels: first the top-level areas, then the pages of each area, with up to 15 original pages behind each new page so their content is kept. A few pages are written at the same time; a Wiki of about 1,000 pages takes roughly 30 to 70 minutes. The tile shows a red bubble when the result is ready.
3. **Review.** The new tree with a preview of each page, **Source Coverage** (where each original page's content went now, or that the architecture documentation covers it) and **About This Documentation**. The new tree stands on its own: links between old pages point to the new pages that hold that content, and nothing links back into the old Wiki. Existing pages are never changed or moved; delete them yourself once the new tree is in use.
4. **Running it again** proposes changes to the existing tree: changed pages side by side, new pages, and pages that are no longer part of the structure (flagged, never deleted).

The method comes from the **Project Management Documentation** persona.

## Debate Architecture

Available once **`<project> Architecture Documentation`** exists. Describe a decision or argue your case. Amy answers as a principal engineer, grounded in the documentation, its differences page and the decision records: a position, real options compared on the same quality attributes, the standards behind each argument, a pre-mortem, and what would change its mind, with links to the pages it used. It changes its position only on new evidence and says so.

- **Mode:** **Advise** (Amy's own recommendation) or **Challenge me** (Amy argues against your proposal on purpose).
- When Amy needs code, it reads specific files with your permissions and lists them under **Read from the code**.
- The method comes from the **Architecture Debate** persona (the Grand Master method by default). Debates use the model set for them (or your own choice), with thinking on unless the persona's effort or your setting says otherwise.
- **Save as decision record (ADR)…** drafts a record in the MADR format under **9 Architecture Decisions**; edit it and select **Create page**.

## Review a Pull Request

On the **Code** tab. Available once **`<project> Architecture Documentation`** exists, like Debate Architecture. The review is **read-only**: it is shown in Amy only. Amy doesn't comment, vote or change anything on the pull request, and needs no extra permission.

1. **Choose the pull request.** The list shows the project's active pull requests: **I'm a reviewer**, **Created by me** or **All active**, optionally for one repository. To review a pull request of another project in the organization, paste its link.
2. **Look for:** bugs and logic, security, reliability and performance, tests, readability and maintainability, whether the changes meet the linked work items, and whether they fit the architecture documentation. **Anything Amy should know** shifts the emphasis.
3. **What Amy reads** lists what is sent: both versions of each changed file, with values that look like keys, passwords or connection strings masked; the title, description, branches and commit messages; the linked work items with their acceptance criteria; and the most relevant passages of the documentation. Lock files, secrets files, binaries, dependencies and generated files are skipped and listed. Existing comments are not read. For large pull requests, code comes before tests and documentation, up to the size budget.
4. **Review** takes a minute or two. The result shows a verdict, a summary, each acceptance criterion as **Met**, **Partly**, **Not met** or **Not in this pull request**, and the findings by severity: **Must fix**, **Should fix** and **Consider**. Each finding shows the code, the reason, a suggested change, what it is based on, and a link that opens the file at that line in the pull request. Findings whose file, line or code is not in the pull request are left out, and Amy says how many.
5. **Copy as Markdown** (one finding or the whole review) to share it, for example as a comment you post yourself. The review is not stored; it stays until you start another one or leave Amy.

The method comes from the **Code Review** persona. Reviews use the model set for them (or your own choice), with thinking on unless the persona's effort or your setting says otherwise.

## Create Sprint Schedule

1. Choose the area in **Board (team)**, the **Generation period** (6, 12 or 24 months) and the **Sprint duration** in whole weeks. Sprints are named after the area (its last segment) and added to the board of the team whose default area is the selected area or its closest parent; the team is shown under the list.
2. Check the **First sprint start date** (default: the day after the area's current sprint ends, or the team's when the area has no sprints of its own) and the **Starting sprint number** (default: the next number when every existing sprint name has one; 1 for an area without sprints). An area's sprints are all the team's sprints when the team's default area is that area, otherwise the team's sprints named after the area. When Amy cannot tell, these fields are empty and must be filled.
3. Choose the naming convention, `<area> <sprint number> <sprint start date> - <sprint end date>` (for example `DevOps 14 28.03.27 - 11.04.27`, dates as DD.MM.YY) or `<area> <sprint number>`. The example name updates as you type.
4. Select **Preview schedule**. Every sprint is listed with number, name, start and end date. End dates are inclusive: a two-week sprint starting on a Monday ends on the Sunday 13 days later. Sprints keep their full length; one that ends after the period is marked **Extends**.
5. Conflicts (a name that already exists, dates overlapping one of the area's sprints) must be resolved by changing the settings; existing sprints are never changed.
6. Tick the approval and select **Create sprints**. Amy creates each sprint as a project iteration and adds it to the team. The result lists each sprint; **Retry** creates or assigns only the unfinished ones.

Creating iterations needs the Azure DevOps permission to edit the project's iterations, and adding them to a team needs team administration rights.

## Project Templates

Lists the templates Amy uses in the current project: built-in Wiki page templates and the Story and Bug outlines, plus the project's own templates.

- **Edit** a template or create a **New template**, then **Review changes** to compare the current and proposed version side by side.
- **Approve and save** stores it, with a higher version number, for everyone in the project.
- **Remove** a project version to return to the built-in template.
- If someone else saved meanwhile, nothing is overwritten; reload and try again.

## Amy on every work item

Open a work item and find the **Amy** group on the form.

**Fields.** Each supported text field has its own row: Description, Acceptance Criteria, Repro Steps, and fields named "Technical Implementation". The controls are in the row's upper-right corner:

| Field | Controls |
| --- | --- |
| Empty | **Generate with Amy**: drafts content for the field |
| Has content | **Improve with Amy**: refines the existing text; **Regenerate with Amy**: writes a completely new version without using the current text |

Each control affects only its field. The proposal is shown as **Current | Proposed**. **Apply to …** writes it into the open form; save the work item yourself to keep it. **Request another version** and **Cancel** are always available, and the current text stays until you apply. Story and Bug templates of the project shape the drafts.

Azure DevOps does not let extensions place buttons inside its own fields, so the controls are in the Amy group rather than on the fields themselves.

**More actions:** split a large story, generate technical Tasks, **Generate Test Cases** (choose the categories), generate test scenarios, identify missing requirements, convert notes into a structured story, summarize comments and discussion, and translate into management-friendly wording. Actions the work-item type cannot support are disabled.

**Field changes** are shown per field as **Current | Proposed**. Accept or reject each one, then select **Apply to form**. The values go into the open form; save the work item yourself to keep them.

**Child items** (split stories, Tasks) are listed with checkboxes:

- Select **Create … child item(s)** to create the selected ones under the current work item, using its Area and Iteration Path. Test Cases are linked to it with "Tested By" instead.
- This requires a saved work item, and more than 10 items need a confirmation.

Summaries and findings are shown as text, with a copy button.

## 6. Publish a Backlog Summary

After work items have been created, select **Publish a summary page** (or the **Publish a Backlog Summary** quick action). Amy proposes a new sub-page of the source page (for example `/Product/Shipment Tracking Portal/Generated backlog 2026-10-09 08-30`). The page lists:

- the target settings
- the backlog summary
- every created or reused work item, with links
- anything that was not created
- the AI assumptions and recommendations

Edit the path and content as needed, then select **Create Wiki page**. If a page already exists at that path, nothing is written; choose another path.

Every created item carries the `Amy-AI` tag and a History entry that names the source Wiki page. A request can create at most 25 items.
