# User guide

Open a project in Azure DevOps and select **Amy** in the project sidebar, next to Boards, Repos, Pipelines, Test Plans and Artifacts. The header shows the current project and whether AI is available.

**Before AI features work:**

1. **Backend configured.** An organization administrator must have connected a backend that your organization operates ([ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md)). Otherwise you see "AI features are not configured for this Azure DevOps organization". Loading and editing Wiki pages and creating work items still work.
2. **Confirm the backend.** The first time, and after any change, confirm the backend your content is sent to. Amy shows its name and hostname.
3. **Sign in.** Select **Sign in**. A pop-up uses your organization's Microsoft account; allow pop-ups for Azure DevOps. The sign-in lasts until you reload the page.

Quick actions that your organization's backend does not offer say so on their box.

## The Amy hub

From top to bottom: the header with Amy's logo (AI settings) and the connection status (select it for details), four tabs, the **Wiki scope** filter (on the Wiki tab only), the quick action cards, and the workspace with the conversation.

| Tab | Quick actions |
| --- | --- |
| **Wiki** | Ask or Summarize Wiki, Create a Wiki Page, Edit a Wiki Page |
| **Work Items** | Create Work Items, Generate Backlog Hierarchy |
| **Documentation** | Review Pull Request Documentation, Generate Release or Sprint Documentation |
| **Others** | Bulk Operations, Project Templates, Create Sprint Schedule |

Amy opens with **Ask or Summarize Wiki**. Selecting a tab shows its cards; selecting a card starts its task, and the cards collapse into a bar with the task's name and **Change action**. Drafts are kept when you switch tasks and come back.

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

**Approval.** Everything that creates or changes content follows **Generate → Review → Approve → Save**:

- Existing content stays as it is until you approve its replacement.
- You can apply a proposal, request another version, or cancel.
- In bulk workflows you review each change and approve selected changes together. Only approved changes are saved.

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

## Ask or Summarize Wiki

Choose the Wiki scope, then type a question and select **Send**, or select **Summarize**. There is no separate question field and no page to load: the scope filter decides what is read (at most 40 pages). In a larger scope, a question reads the pages Azure DevOps Wiki Search finds for its keywords (the result says "found by Wiki search"); without the Search extension or a match, the first 40 pages are read and the result says the scope was cut.

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

**Results.** **View results** shows the documentation impact and the pages Amy proposes to create or update, or that no updates are needed. Proposals open in **Pending proposals**:

- **Update** proposals open side by side (**Current version | Proposed version**). Reject individual changes if needed, then select **Approve Changes**.
- **New page** proposals (release notes, upgrade and migration instructions) show a preview first, then **Approve Changes**.
- Select several proposals and choose **Approve selected** to approve them together. **Dismiss** removes a proposal without changing the Wiki.

Before saving, Amy checks that the page has not changed since the proposal was prepared. If it has, Amy prepares the proposal again against the latest version and asks you to approve it again. When several pull requests affect the same page, they are combined in its pending proposal; every new revision needs a fresh approval.

**Automatic checks.** Completed pull requests in repositories an administrator enabled are also checked right after completion, whether or not Amy is open. Amy then @mentions the configured reviewers on the pull request; if nothing needs documenting, nobody is notified.

Without automatic checks on your organization's backend, **Analyze** runs in Amy: the results show the impact, the affected pages (with **Add PR link to page**, reviewed side by side) and draft release notes, upgrade and migration instructions.

Proposals you requested from an AI client through Amy's MCP server appear in the same list, labeled "Requested by you through an AI client". Only you see them.

## Generate Release or Sprint Documentation

1. Choose the documentation type: sprint documentation (default), release notes, deployment checklist, testing summary, known limitations, rollback plan, support handover, and more.
2. Choose the **Board** (team) and **Sprint**; the current sprint is preselected.
3. Select **Generate Sprint Documentation**. Amy lists the completed work items of the sprint and drafts the document.
4. Review and edit the page. Sprint documentation is named after the sprint (for example "Sprint 12"), other types "Sprint 12 – Release notes". Select **Create Wiki page** to approve its creation.

Selecting a release instead of a sprint is not available yet. To create the sprints themselves, use **Create Sprint Schedule**.

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

## Bulk operations

Select **Others > Bulk Operations**.

1. Find work items by type, state, title text or IDs, and select up to 25.
2. Choose an action:
   - **Generate Tasks for selected User Stories:** Tasks are proposed per story. Untick any you don't want; the rest are created under their story, in the story's Area Path.
   - **Generate acceptance criteria for selected draft items:** shows current | proposed criteria per item. Updates are rejected for any item that someone changed after the preview was made.
   - **Generate Test Cases for selected User Stories:** Test Cases from each story's acceptance criteria. The ones you keep are created and linked to their story with "Tested By".
   - **Improve descriptions of selected work items:** current | proposed description per item; same concurrency check as acceptance criteria.
   - **Create Wiki summaries for selected Features:** one new page per Feature under the parent path you choose. It lists the description, acceptance criteria and child work items. Existing pages are skipped, never overwritten.
   - **Link selected work items to a Wiki page:** adds a hyperlink to the chosen page. Items that already have the link are skipped.
   - **Generate one release or sprint page from selected items:** choose the document type and enter a release or sprint name. Use the **Iteration Path** filter to select a sprint's items.
     - Document types: sprint scope, release notes, deployment checklist, known limitations, testing summary, support handover, operational readiness, rollback plan, post-release validation.
     - The draft links every work item it mentions. Work items that were not selected cannot appear in it.
     - Missing information is marked TODO. Edit the draft, then create it as a new page; existing pages are never overwritten.
3. Select **Preview**, untick any entries you don't want, confirm if there are more than 10, and select **Run**. The results list each entry as created, updated, skipped or failed.

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
