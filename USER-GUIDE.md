# User guide

Open a project in Azure DevOps and select **Amy** in the project sidebar, next to Boards, Repos, Pipelines, Test Plans and Artifacts. The header shows the current project and whether AI is available.

**Before AI features work:**

1. **Backend configured.** An organization administrator must have connected a backend that your organization operates ([ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md)). Otherwise you see "AI features are not configured for this Azure DevOps organization". Loading and editing Wiki pages and creating work items still work.
2. **Confirm the backend.** The first time, and after any change, confirm the backend your content is sent to. Amy shows its name and hostname.
3. **No separate sign-in, usually.** Amy works with your Azure DevOps sign-in: the backend confirms who you are with Azure DevOps and renews the session in the background. If your organization chose a Microsoft sign-in instead, select **Sign in**: a pop-up uses your organization's Microsoft account (allow pop-ups for Azure DevOps), closes by itself when you are already signed in to Microsoft, and lasts for this browser tab until it expires (an hour by default) or you close the tab.

Quick actions that your organization's backend does not offer say so on their box.

## The Amy hub

From top to bottom: the header with Amy's logo (AI settings) and the connection status (select it for details), seven tabs, the **Wiki scope** filter (on the Wiki tab only), the quick action cards, and the workspace with the conversation.

| Tab | Quick actions |
| --- | --- |
| **Wiki** | Ask Amy, Create a Wiki Page, Edit a Wiki Page |
| **Work Items** | Ask about Work Items, Create Work Items (From a prompt, From a Wiki page) |
| **Documentation** | Review Pull Request Documentation, Generate Release or Sprint Documentation, Generate Project Documentation, Debate Architecture |
| **Code** | Review a Pull Request, Code with Amy (organization administrators and the teams they allow), Repository Documentation (Check, Generate) |
| **Support** | Diagnose a Ticket |
| **Requirements** | Design Requirements |
| **Others** | Project Templates, Create Sprint Schedule |

Your organization may limit tabs to some teams (Organization settings > Amy AI > Access by tab). Then you see only your tabs; if you need another one, ask an administrator. The chat stays with Ask Amy or Ask about Work Items, whichever tab you have, and searches only the sources of your tabs.

Amy opens with **Ask Amy**. Selecting a tab shows its cards; selecting a card starts its task, and the cards collapse into a bar with the task's name and **Change action**. Drafts are kept when you switch tasks and come back.

**From the project sidebar.** Hover over **Amy AI** in the project sidebar to see the five categories; select one to open Amy on that tab, with its quick actions. **Wiki** opens Amy as described above. Inside Amy, the tabs keep working as before; switching tabs there keeps your drafts and conversation, while selecting another category in the sidebar opens Amy again.

**Wiki scope.** Questions and summaries use the active Wiki of the project (switch it next to the filter if the project has several):

- **Entire Wiki:** all its pages.
- **Selected pages:** search and tick one or more pages.

The **Wiki Scope Preview** beside the conversation shows "Entire wiki selected", the selected page, or the selected pages to open one by one. **Hide** collapses it.

Every conversation with Amy (Ask Amy, Amy's designer, Design Requirements, the debate, Revise with Amy) follows what she adds, her steps included: a long answer shows from its start. If you scroll up to read while she works, the conversation stays where you are and **Show the latest** takes you back.

**Chat.** The composer, **Ask, summarize Wiki or describe what you need**, belongs to Ask or Summarize Wiki and is shown only while that task is open. It does three things:

- A question with **Send** is answered from the Wiki scope, in the conversation, with links to the supporting pages.
- **Summarize** summarizes the Wiki scope in the conversation. Typed text, if any, is the focus. **Publish as Wiki page…** shows the page for review; it is created only when you approve.
- Other requests start the matching task with your text filled in, for example "Create a user story for CSV export", "Find duplicates of the tracking feature" or "Check the documentation quality of this page". Amy tells you which task it started. A request for a backlog, or for work items from a page ("Create user stories from the portal page"), opens Create Work Items on its **From a Wiki page** tab.

**Wiki Only Answer Mode**, beneath the composer's buttons: answers use only the selected Wiki content and say so when it is not enough. Switched off, general knowledge is shown separately from what the Wiki supports. It starts off where your organization's backend has web search (which is then on, and the two exclude each other), and on otherwise. Amy never saves anything from a chat message alone.

**AI settings.** Select Amy's logo, left of the title, to open **AI Settings** beneath it. Changes apply right away to your next requests; there is no Save button, and requests already running keep the settings they started with. The settings last for this conversation (until Amy is reloaded) and close with Escape, a click elsewhere or the logo again.

| Setting | What it does |
| --- | --- |
| **Model** | The models your organization approved, marked Local (its own infrastructure) or Cloud. The default is the organization's configured model. |
| **Thinking** | On: the model reasons before answering (slower, often more thorough). Available only for models that can switch it; otherwise it is disabled with an explanation. A new model starts with its own default. |
| **Session memory** | On by default. Amy remembers the chat's earlier questions, answers and summaries (the last six, answers shortened), so follow-ups such as "and who approves it?" work; a follow-up is searched in the Wiki together with the question before it. Leaving the chat (another task or tab) ends the session: the conversation and Amy's memory of it are cleared. Off: each question is answered on its own. Earlier answers are context only; answers still cite the Wiki. |
| **Response length** | Short (brief and direct), Standard (balanced, default) or Large (detailed). It changes the wording only: required fields, items, citations and structured results are never cut. |
| **Web search** | Available only when your organization's backend has a search integration, and then on by default. On: a question searches the web when the Wiki does not answer it, and always when you ask Amy to search the web; **Create Work Items**, **Design Requirements**, **Review a Pull Request** and **Debate Architecture** search when the work involves outside products, services, libraries, standards or regulations, or when you ask for it, and show **From the web** with the search terms and the pages used; **Ask with search** and **Diagnose a Ticket** may search for public facts. Off, none of them searches. Since 0.14.2 your question is never sent as typed: Amy writes public search terms from it, and never sends a term that names your organization, project, repositories or services, or holds an address, a file path, an ID or personal data. Only search terms are sent to the search service: your question, or up to three terms Amy writes from your request with public names only, never your pages, and never anything that looks like a key, password, token or connection-string secret (Amy then works without the web and says why). Web findings appear separately, "From the web (not from the Wiki)", with links to their sources. Turning web search on turns **Wiki Only Answer Mode** off; turning Wiki Only Answer Mode on turns web search off. |

The settings never change your permissions, the Wiki scope or the approval steps; the backend checks them on every request.

**Model per quick action.** The **Model** list starts at **Set by your organization**: each quick action then uses the model and effort your organization's administrators set for it. Choose a model (and **Thinking**) to use it in every quick action instead. A quick action your organization **locked** always uses its own model; the menu shows 🔒 and which model applies.

**Admin settings.** The last entry of the menu, **Admin settings**, opens **Organization settings > Amy AI** in a new browser tab, with tabs for **Backend settings** (the organization's AI backend), **Personas** (when the backend offers them), **Pull-request documentation checks** (when the backend offers them) and **Repository documentation** (when the backend offers it: which repositories get documentation and who reviews its pull requests). Azure DevOps decides who can open Organization settings. Administrators (Project Collection Administrators, or people your backend operator gave the admin role) also see **Monitoring**: a table of how often each quick action ran in the last 7, 30 or 90 days. One question, generation, review or documentation job counts once; only counts are kept, no content and no names.

**Personas (administrators).** Under **Organization settings > Amy AI > Personas**, for the whole organization:
- the **identity** (who Amy is: voice and house rules; `{project}` is replaced by the current project's name), and
- one persona per quick action (Ask and Summarize, Create a Wiki Page, Edit and Review Wiki Pages, Create Work Items and Backlogs, Work Item Improvements, Test Cases, Pull Request Documentation, Release and Sprint Documentation, Architecture Documentation, Architecture Debate, Code Review, Project Management Documentation, Update the Wiki, Check Repository Documentation, Repository Documentation, Design Requirements), each with its text, a **Default model**, a **Default effort** (Model default, Off, Low, Medium, High; only what the model supports) and **Lock** (users can't choose another model or thinking for it).

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

Tasks that work on one page (Edit a Wiki Page, Create Work Items from a Wiki page, documentation quality review) start with a page search in the active Wiki; Create Work Items' **From a Wiki page** tab, on the Work Items tab, has its own **Wiki** selector when the project has several. Type part of the name or path and pick the page; its content is shown right away. If one page is selected in the Wiki scope, it is preselected.

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
2. Select the **work-item types** to generate. The highest selected type gets exactly one item, and every other item sits below it: with Epic, Feature, User Story and Task, one Epic with its Features, their stories and tasks. Types your project's process does not have are disabled, with an explanation. Some types are mapped instead, for example "User Story" is created as "Product Backlog Item" in Scrum or as "Issue" in Basic.
3. When the page has subpages, **Include its N subpages** sends them along, read with your permissions: as many as fit in one request (200,000 characters, at most 100 subpages), in the Wiki's order, each under a heading with its name and path. Under the button, Amy then names the subpages it left out or couldn't read; choose a lower page to cover the rest.
4. Select **Generate proposal**. This can take up to a minute.

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

**With search (when your organization set it up).** Amy searches the whole index of your Wikis, code and work items, the way an engineer works through a repository: it searches, reads only what matters and follows leads, such as an error code to the code that raises it, or a file to the bug that changed it. With Claude as your backend's AI provider, Amy works through her tools in one go, keeping her reasoning from step to step: she opens the files the code imports or calls by their path, reads the lines that matter of long files, and checks before answering that every source she cites is one she read. While she works, a card shows the step she is on, her phase (Search, Read, Answer), the time so far and **Stop**, above a trail of what she searched and opened; the files, Wiki pages and work items in it are links. When a question needs a second round of searching and reading, the card says **Deep research** and glows while she works (with reduced motion, it keeps a still blue border). **Stop** ends the question without an answer; ask again to start over. When she's done, the card shows the totals (searches, sources read, time), and **How Amy got there** opens the trail. The answer cites its sources as [S1], lists them with links, and for questions about code or bugs says **Where to start**. It reads at most 30 work items in full per question, however many there are. When it can't answer from the sources, you get **Insufficient evidence**; with **Sources Only Answer Mode** off it may add general knowledge and search the web. Selected pages and page-only mode still read exactly those pages. A chat has no limit on questions, and with session memory on Amy keeps the whole chat in mind (in a very long chat, the oldest part in short). **New question** clears the conversation and Amy's memory of it.

**Attached files (with search).** Select **Attach files**, or paste a screenshot into the message box, to send up to 5 files with a question: logs, text exports (CSV, JSON, XML, HAR), e-mails (.eml) and screenshots (PNG, JPEG, WebP, made smaller in the browser). Other files, such as PDF or Word documents, are refused with the reason. A text file over 400,000 characters keeps its start and its end. Amy reads the files with your question and follows the error codes, times and IDs in them; a screenshot is read once, and Amy works from what it wrote down (shown with the answer). With session memory on, later questions in the chat still have the files, until **New question**. Personal data in text files is masked like everything else, but **screenshots go to the AI model as they are**: crop or leave out screenshots that show data the model must not see.

**Sources.** Above the scope filter, turn **Wiki**, **Work items** and **Repos** on or off; a source is green while it is on, and one always stays on. **Repos** needs the search index; with it, all three are on until you change them. **Ask about Work Items** (Work Items tab) opens the same chat with only Work items on. Without the search index, with work items among the sources:

- A **work-item filter** appears: **Time frame** (any time, last 30 days, this or last quarter, this year, custom range), **Date means** (completed, created or changed; completed by default), **Types** and **Area**. The time frame applies to work items only; with Both, the Wiki part is never filtered by date. When your question names a period ("in Q3", "last year"), Amy uses it and updates the filter.
- Overview questions ("a structured overview of everything we finished in Q3") get themes grouped by Epic with the items of each. Topic questions ("did we ever implement single sign-on with Entra?") search for the subject and related terms (shown in the answer) and get a verdict (implemented, partly, planned, not found), confidence and a table of the work items used, each linked and with the quoted evidence. Wiki sources are listed separately.
- Each answer starts with the filter it used; **Change filter** jumps to it. At most 300 items are read per question; the answer says when more matched.
- When related items share no Epic, **Connect them under a new Epic…** proposes one (title, area, description) and shows the link each item gets. Existing parents never change: Features without a parent become children of the Epic, other items get a Related link, items already under one of them need none. Nothing is created until you confirm.
- **Publish as Wiki page…** creates the answer as a new page, with a link to every work item, after you approve it.
- The checkbox reads **Sources Only Answer Mode**. Amy sees only work items you can read.

**Wiki.** Choose the Wiki scope, then type a question and select **Send**, or select **Summarize**. There is no separate question field and no page to load: the scope filter decides what is read (at most 40 pages). In a larger scope, Amy first asks the AI for search terms (the question's subjects with synonyms, abbreviations, spelled-out forms and English translations) and reads the pages Azure DevOps Wiki Search finds for them and the question's keywords; the result says "found by Wiki search" and lists the terms. Without the Search extension or a match, the first 40 pages are read and the result says the scope was cut. Pages are read six at a time.

Each answer lists its sources: page link, section heading, Wiki and the exact supporting passage, with a confidence level. If the pages don't contain the answer, you get **Insufficient evidence** instead of a guess. With **Wiki Only Answer Mode** off, general background appears separately as "General knowledge (not from the Wiki)". Retrieved Wiki content is treated as source material, never as instructions to Amy.

## Create a Wiki Page

1. Optionally choose a project template (for example Deployment guide).
2. Enter the page title and what the page should cover.
3. Optionally use the pages selected in the Wiki scope and work items (by ID) as sources.
4. Select **Draft page**. Amy first lists existing pages with a similar name; continue only if none of them should be edited instead.
5. The review shows the **destination Wiki** (the active Wiki; changeable here if the project has several), the title and location, the content and any similar pages. Edit what you need and select **Create Wiki page**. Existing pages are never overwritten. **Request another version** drafts it again.

## Update the Wiki

One change applied to every page it affects, for example "The new project manager is Mike Brown" or "The team now has only these roles: operations manager, project manager, application owner, product owner". Available when your organization's backend offers it.

Everyone with the Wiki tab sees every update: who started it, the change it was asked for and how far it is. No two run on the same Wiki at once: while one is under way, or waits for decisions (for up to a day), starting another says who holds the Wiki. Only the person who started an update or an administrator can open what it found, decide on it, retry it or discard it.

1. **Describe the change** under **What changed?** and choose the pages of the active Wiki: **All pages**, or **Selected pages** (search, pick several, **Include their subpages** for whole branches). Amy searches the pages for the names and roles the change concerns, also in their other spellings and languages, and sends only those pages to the AI; **Let the AI read every page** checks every page instead (slower, more tokens; the estimate is shown). Select **Find affected pages**. The job runs on your organization's backend: you can close Amy, and the tile shows a red bubble when your decisions are needed.
2. **Your decisions.** Amy shows the change as it understood it, the affected pages with the reason for each, and questions about what it can't know, for example who takes over the duties of a role that no longer exists. Pick an answer or write another one; a question left unanswered makes the AI leave what it concerns unchanged. Records of the past (meeting minutes, decision records, release notes) are listed but not chosen: they stay as they were unless you choose them. Select **Propose changes for N pages** (up to 300).
3. **Review.** Each page shows its proposed edits; **Review changes** compares them side by side, and you accept or reject each one before **Save this page**. **Save N pages** saves the chosen pages with all their edits. The edits are placed on each page as it is now: an edit whose text someone changed in the meantime is left out, and a page is never overwritten. The previous versions stay in each page's history. **Discard** deletes the stored result.

## Create Work Items

One card with two tabs, like Generate Project Documentation. Each tab keeps its draft when you switch, and both lead to the same review.

- **From a prompt:** enter a **title** and a **short description**, choose the work-item types and select **Generate proposal**.
- **From a Wiki page** (formerly Generate Backlog Hierarchy): choose a page, optionally with its subpages, and generate the hierarchy as in sections 1 and 2.

**Web search.** With web search on (Amy settings; on by default where your organization's backend has it), Amy first decides whether the work needs public facts the source doesn't give: a third-party product, service, API or library, a standard, a regulation, a platform's requirements. If so, or if your instruction or description asks it to search the web, it searches for up to three terms it writes from the request (public names only; your page or description is never sent) and uses the results only for those facts: never for scope. The review shows **From the web, not from the Wiki** with the search terms and the pages used, and items that rely on a page say **Web: 1 page**; their description links the pages under **Web sources**, and so does the backlog summary page. Regenerating one item doesn't search.

The proposal opens in the review (sections 3 to 5). Possible duplicates are checked before creation. If your organization uses Amy's MCP server, proposals you requested from an AI client are listed below both tabs (**From your AI clients**); load one to review, edit and create its items. In **Monitoring**, the two tabs are counted separately: "Create Work Items: from a prompt" and "Create Work Items: from a Wiki page" (which includes the earlier Generate Backlog Hierarchy counts).

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

An administrator can also have these documents prepared by themselves at the end of every sprint ([ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md#sprint-documentation)). They then wait under **Review Pull Request Documentation**, origin "End of sprint", until someone approves them.

## Generate Project Documentation

Select **Documentation > Generate Project Documentation**. Your organization's backend must offer it; it is off unless an administrator enables it, because source code and Wiki content are sent to the backend and its AI provider. Two tabs, one job each (both can run and wait for review at the same time). Everyone with the tab sees every job: who started it and how far it is. No two of the same kind run for a project at once: while one is under way, starting another says who holds it. Only the person who started a job or an administrator can open its result, retry it or discard it.

- **Create architecture documentation**: from the repositories and the Wiki, an arc42 tree under **`<project> Architecture Documentation`**, described below.
- **Create project management documentation**: the content of this project's Wiki, and of other projects' Wikis if you choose them, reorganized into a new tree under **`<project> Management Documentation`** in this project's Wiki, described after it.

The two trees don't repeat each other: the architecture documentation leaves out project management topics (roadmap, team, way of working, meetings) and links the management documentation instead, and the other way around for architecture topics (system structure, components, infrastructure, technical decisions).

### Create architecture documentation

1. **Start.** Choose the repositories (default branch; up to 300, **Select all** and **Clear** help). Under **Projects**, add other projects whose repositories belong to the same system, for example deployment or infrastructure projects; their repositories are then named "repository (project)" in the documentation, which is still created in the current project's Wiki. Their Wikis are listed under **Wikis**, to compare the code with: a project's Wiki is chosen when you add the project, code Wikis (published from a repository) only when you check them, and you can leave out any Wiki, this project's included. Under each chosen Wiki, **All pages** reads it whole, while **Selected pages** reads only the pages you find and pick (**Include their subpages** for whole branches). You see only repositories and Wikis you can read. Each repository shows its last commit; one without a commit and without a successful build in 12 months is marked **possibly retired** and starts unchecked (check it to document it anyway), and "About This Documentation" lists those left out.
   - **Services.** Amy finds the services in the file tree: a folder with a Dockerfile and code, an Azure Functions app, or a project file next to its entry point (for example a `.csproj` with `Program.cs`). A repository with one service, or none found, is one service with all its files. Programs of the same service in one repository, whose names differ only by words such as Api, Worker, Functions, Host, Migrations or Jobs (for example `Orders.Api` and `Orders.Worker`), are merged into one service named after them (`Orders`): their rows show **Part of** filled in and "Suggested", and setting it back to "—" keeps one apart; when the whole repository becomes one service this way, its other files join it. In a repository with several services, files outside them form "<repository> (shared)", and deployment files (Helm charts, Kubernetes, infrastructure, pipelines) join the service whose name a folder matches. A generation takes up to 500 services and shared parts together, because each gets its own page. A service whose analysis doesn't pass Amy's checks, even after a repair and a second attempt, is left out instead of stopping the generation: "About This Documentation" lists it under **Not analyzed**, and the next generation tries it again. The later steps work the same way: answers are completed where they can be (a service the AI forgot joins its closest domain, an unusable diagram is left out), a comparison with the Wiki that keeps failing the checks lists no differences (**Not compared with the Wiki**), and a page that keeps failing them is written from the analysis without the AI (**Written without AI**). Source references the AI got wrong are removed rather than causing a failure, and so is evidence for an interface, data store or setting whose cited code doesn't name it (an item left without evidence is left out). The **Services** table lists them: clear **Include** to leave one out, rename it, mark it as **Shared code or infrastructure**, merge it into another with **Part of**, or choose **All files** to read it completely. Your changes to the table are saved for the project when you start, and the next generation, yours or a teammate's, starts from them.
   - **Key files.** Each service gets a fair share of its most informative files, code first, because READMEs and API definitions are often out of date: project files, deployment, pipelines and configuration, entry points and the source that defines its behavior (controllers, endpoints, handlers, functions and triggers, message consumers and publishers, clients of other services, repositories, data contexts), then API definitions and the newest migrations, other source across its folders, and documents last, within 5% of its share. With few services the share is larger (about 800 files for a single one); with many, at least 30 files each. Up to 8,000 files per generation.
   - **Facts found in the code.** When you start, Amy also reads every other file of the chosen services that its patterns understand (C#, TypeScript and JavaScript, Java and Kotlin, Python, Dockerfiles, Compose files, launch profiles and appsettings; files up to 200 KB, 60 million characters in all), shown as "Reading code for facts", and finds endpoints, triggers, outgoing calls, events published and consumed, connection strings, settings, environment variables, schedules and ports, each with its line. Only these facts are uploaded for those files, not their content. The analysis treats them as true, they reveal calls and events between services, and each service page lists them under **Found in the code**. Names built at run time are not found.
   - Amy shows what it reads: files found and selected, with **Show selected files** for the details and what is not uploaded (dependencies and build output, binaries, lock files, secrets files, generated files; tests are listed only). Classic build and release pipelines are read too. It compares with the chosen Wiki pages, up to 2,000 in all (this project's Wiki first), and estimates time and tokens. Values that look like keys, passwords or connection strings are masked before upload.
   - **Team facts.** Once the documentation exists, create the page **Architecture Facts** below its main page and write down what the team knows for certain, for example "Billing owns all invoices" or "LegacyGateway is retired". Amy reads it on every run, shows how many lines it applies, and every step treats them as true: where the code disagrees, the pages say so. Amy never changes the page.
   - **Checked repository documents.** For services checked with **Repository Documentation > Check** (Code tab), the analysis leaves their documents (READMEs, docs, API definitions) out and never uses what the latest check found the code contradicts; "About This Documentation" names the date of the check. Dismissed conflicts don't count.
   - **Runtime data** (optional). Under **Runtime data**, copy the two queries, run them in the Azure portal (Application Insights Logs for the calls of the last 30 days, Azure Resource Graph for the deployed apps), export the results as CSV and add the files. The dependency tables of the service pages then show for each call between services whether it was observed (**in code and observed**, **in code, no traffic in 30 days**, **observed, not found in code**), services that no deployed app matches are flagged, and "About This Documentation" sums it up. Only the rows of the files are sent, with the job.
2. **Main page.** The tree goes under **`<project> Architecture Documentation`** in the project Wiki. If a page with that name exists that Amy did not create, enter another name; Amy never changes pages it did not generate.
3. **Running.** The job runs on your organization's backend in four steps (analyze the code, build the system model, compare with the Wiki, write pages), a few AI calls at a time. Services whose files did not change since the last run are not analyzed again. You can close Amy meanwhile. When it is done, the card shows a small red bubble and the Documentation tab a dot; they clear when you open the result. A failed job shows the bubble too; **Retry from step N** continues where it stopped.
4. **Review.** The page tree follows arc42: the main page with a context diagram, 1 Introduction and Goals through 12 Glossary, a subpage per building block, runtime flow and crosscutting concept, where the building blocks are domains (groups of services; with 12 services or fewer, each service is its own) and each domain has a subpage per service. Service pages end with **Dependencies**: the services it calls, that call it, and the events it publishes and consumes, with a diagram and the code that shows each one, and, where there is any, **Documented, not confirmed in code**: interfaces, data stores, settings and deployment facts that only the service's documents (README, docs, API definitions) state, to check before relying on them; domain pages list their services, and **5 Building Blocks** has an index of all services. Then **Code vs. Wiki Differences** (contradictions, parts missing in the Wiki, Wiki content not found in the code) and **About This Documentation**. The Wiki pages the documentation cites are copied below **Wiki Sources** (the most cited first, as many as fit), each with a note naming its original, and the documentation links to these copies instead of the original pages: in a copy, links to other copied pages lead to their copies, other links become plain text, and pictures of this project's Wiki stay. Everyone who can read this project's Wiki can read the copies, also those of other projects' pages. Code references link to the analyzed commit; diagrams are Mermaid. Clear a box to leave a page out, then **Create N pages**. Creating or **Discard** deletes the stored result. Below the pages, **Differences from the Wiki** lists the differences found: **Not a difference** with a reason dismisses one for the whole team, and later generations leave it out (the differences page says how many).
5. **Running it again** proposes changes to the existing tree only: changed pages side by side (accept or reject each change), new pages, and pages of parts that left the code (a note is added; pages are never deleted). **All pages** lists the whole documentation, unchanged pages too, to read it again. Copies below **Wiki Sources** are yours to edit: a later run leaves a copy as it is while it copies the same page, adds copies of newly cited pages, and flags copies no longer cited.

### Create project management documentation

1. **Choose the Wikis.** This project's Wiki is chosen. Under **Projects**, add projects whose Wikis belong to the same product or team: their project Wiki is chosen when you add them, code Wikis (published from a repository) only when you check them. You can also leave this project's Wiki out. The new tree always goes to this project's Wiki; the other Wikis are only read. Their pages appear under `/<project>/<wiki>` in **Source Coverage**, and their links to Wiki pages and attachments become plain text, since those lead nowhere in this project's Wiki. Under each chosen Wiki, **All pages** reads it whole, while **Selected pages** reads only the pages you pick (**Include their subpages** for whole branches).
2. **Start.** Amy shows how many pages it reorganizes: the chosen Wikis except the architecture documentation, the management documentation and old `/Amy` persona pages, up to 2,000 pages in all (more get a warning: only the first 2,000 are included, this project's Wiki first), with an estimate of the time and of the input and output tokens. If a page named `<project> Management Documentation` exists that Amy did not create, enter another name.
3. **Running.** Three steps on your organization's backend: **Understand the pages** (what each page holds), **Design the structure** (a page tree built from the content, closer to how software teams document a project; internal procedures and team-specific topics keep their own sections) and **Write pages**. For a Wiki of more than 150 pages, the structure is designed in two levels: first the top-level areas, then the pages of each area, with up to 15 original pages behind each new page so their content is kept. A few pages are written at the same time; a Wiki of about 1,000 pages takes roughly 30 to 70 minutes. The tile shows a red bubble when the result is ready.
4. **Review.** The new tree with a preview of each page, **Source Coverage** (where each original page's content went now, or that the architecture documentation covers it; for a large Wiki it continues on subpages **Part 2**, **Part 3** and so on) and **About This Documentation**. The new tree stands on its own: links between old pages point to the new pages that hold that content, and nothing links back into the old Wiki. Existing pages are never changed or moved; delete them yourself once the new tree is in use.
5. **Running it again** proposes changes to the existing tree: changed pages side by side, new pages, and pages that are no longer part of the structure (flagged, never deleted). **All pages** lists the whole documentation, unchanged pages too.

The method comes from the **Project Management Documentation** persona.

### Revise with Amy

Under the preview of either documentation (also when a later run proposes changes), **Revise with Amy** changes the result before anything goes to the Wiki. Write what should change, for example "Merge Deployment into Operations", "The API runs on Container Apps, not App Service" or "Move the runbooks below Operations", and select **Send**.

- **Pages that are right.** Add up to 5 pages of this documentation or of the Wiki (**Add page**). Amy reads them with your permissions and takes them as right: where the documentation says something different, it changes the documentation, never those pages. The next change offers the same pages again.
- **What Amy does.** It plans the change (pages to rewrite, add, move, rename or remove, with their subpages), then rewrites each page concerned, up to 25 per change; for a page it changes only the sections concerned, and the rest stays exactly as it was. Links to moved pages follow them, and links to removed pages become their text. When the instruction is unclear, Amy asks back instead of changing anything.
- **Each change runs on your organization's backend** like the generation, with the same persona and model; the chat shows its progress, and **Create** waits until it is done. If a page's new text doesn't pass the checks, nothing changes and Amy says so.
- **Undo the last change** brings the result back to how it was before it. Pages Amy changed in the last change are marked **changed** in the tree.

## Debate Architecture

Available once **`<project> Architecture Documentation`** exists. Describe a decision or argue your case. Amy answers as a principal engineer, grounded in the documentation, its differences page and the decision records: a position, real options compared on the same quality attributes, the standards behind each argument, a pre-mortem, and what would change its mind, with links to the pages it used. It changes its position only on new evidence and says so.

- **Mode:** **Advise** (Amy's own recommendation) or **Challenge me** (Amy argues against your proposal on purpose).
- When Amy needs code, it reads specific files with your permissions and lists them under **Read from the code**.
- **Sources** lists the documentation pages the answer cites. Quotes that aren't found word for word in the documentation are removed. When nothing is cited, Sources says so: check what the answer says about the system.
- The method comes from the **Architecture Debate** persona (the Grand Master method by default). Debates use the model set for them (or your own choice), with thinking on unless the persona's effort or your setting says otherwise.
- **Save as decision record (ADR)…** drafts a record in the MADR format under **9 Architecture Decisions**; edit it and select **Create page**.

## Review a Pull Request

On the **Code** tab. Available once **`<project> Architecture Documentation`** exists, like Debate Architecture. The review is **read-only**: it is shown in Amy only. Amy doesn't comment, vote or change anything on the pull request, and needs no extra permission.

1. **Choose the pull request.** The list shows the project's active pull requests: **I'm a reviewer**, **Created by me** or **All active**, optionally for one repository. To review a pull request of another project in the organization, paste its link.
2. **Look for:** bugs and logic, security, reliability and performance, tests, readability and maintainability, whether the changes meet the linked work items, and whether they fit the architecture documentation. **Anything Amy should know** shifts the emphasis.
3. **What Amy reads** lists what is sent: both versions of each changed file, with values that look like keys, passwords or connection strings masked; the title, description, branches and commit messages; the linked work items with their acceptance criteria; and the most relevant passages of the documentation, together with the repository's own decision records and API reference (the `adr`, `adrs`, `decisions`, `architecture/adr` and `api` folders of its documentation folder, always as they are on the default branch, whatever branch the pull request targets). Lock files, secrets files, binaries, dependencies and generated files are skipped and listed. Existing comments are not read. For large pull requests, code comes before tests and documentation, up to the size budget.
4. **Review** takes a minute or two. The result shows a verdict, a summary, each acceptance criterion as **Met**, **Partly**, **Not met** or **Not in this pull request**, and the findings by severity: **Must fix**, **Should fix** and **Consider**. Each finding shows the code, the reason, a suggested change, what it is based on, and a link that opens the file at that line in the pull request. Findings whose file, line or code is not in the pull request are left out, and Amy says how many.
5. **Copy as Markdown** (one finding or the whole review) to share it, for example as a comment you post yourself. The review is not stored; it stays until you start another one or leave Amy.

The method comes from the **Code Review** persona. Reviews use the model set for them (or your own choice), with thinking on unless the persona's effort or your setting says otherwise.

## Code with Amy

Where your organization's backend offers it, and only for organization administrators and the teams and groups they allow in **Organization settings > Amy AI > Code**. Everyone else doesn't see the card. From a task or a work item to a pull request that is ready for review:

1. **Say what should change.** Describe it, name a work item, or both, and choose up to five repositories. You need Contribute on each of them.
2. **Amy reads the code and writes a plan.** She starts from the repository's own instructions (CLAUDE.md, AGENTS.md, CONTRIBUTING), traces the code and asks you only what changes the solution. A second engineer, who didn't write the plan, checks it against the code; what they said shows above the plan.
3. **You approve the plan, or say what should change.** Nothing is written before you approve. The plan names every file, the tests to write first, what could go wrong and how each acceptance criterion will be verified. If it changes a pipeline or an agent's instructions, a warning says so: those are the only way a change reaches further than the code, and Amy changes them only when the plan or you name them.
4. **Amy writes the tests and the code** on a branch of her own (`amy/code/...`). If the plan proves wrong, she stops and asks.
5. **A reviewer who hasn't seen how it came about checks the change**, and Amy fixes what they find. Tests that were removed, skipped or check less, and files outside the plan, are named to the reviewer and in the pull request, whatever the reason.
6. **A draft pull request opens and your pipelines run it.** Amy can't run code herself: your own pipelines do, under your organization's rules. She reads what failed and fixes it, within the rounds your administrators set. When a failure has nothing to do with the change, or merge conflicts stand in the way, she tells you what she found and waits for you.
7. **It goes to its reviewers once the checks pass.** The pull request says what was verified and, plainly, what wasn't. Amy never pushes to the target branch and never completes a pull request.
8. **After that** you can ask for a change, or have her take up the open review comments; both come back through the review and the checks.

The card shows when something waits for you. **Stop** keeps the run: **Continue** picks it up where it stopped. **Discard** removes the run and abandons its draft pull requests; what is already with its reviewers stays. Colleagues who have Code with Amy see that a run exists and where its pull requests are; only who started it and administrators see its plan and act on it. The run shows what it has used so far, at list prices.

The method comes from the **Code with Amy** persona. Plans, code and reviews use the models set in **Organization settings > Amy AI > Code** at high effort; for what isn't set there, your own choice or the persona's, and otherwise the largest the backend offers.

## Repository Documentation

On the **Code** tab, one card with two tabs, each shown where your organization's backend offers it: **Check** finds where the repositories' documents contradict the code and reports it in this project's Wiki; **Generate** writes the documentation set from the code into each repository and opens a pull request. A red dot on a tab means its result is waiting for you.

### Check

Where your organization's backend offers architecture documentation. It finds where the documents in the repositories (READMEs, docs, OpenAPI and Swagger definitions) contradict the code. The code is taken as true. The only output is one Wiki page, **Code vs. Repository Documentation**, which you review before it is saved: no work items, no pull requests, nothing in the repositories. It needs documents to check: for services that have none, it points you to **Generate**, which writes them from the code.

1. **Choose** the repositories and services, as for the architecture documentation: the same **Projects**, **Repositories** (possibly retired ones start unchecked) and **Services** table, whose changes are saved for the project and shared with the architecture documentation. **What Amy reads** shows the documents to check and the other files to check them against: each service's documents, configuration, manifests and container files first, then as much of its code as the limits allow, plus the facts found in all of its code. Values that look like keys, passwords or connection strings are masked before upload.
2. **Running.** Three steps on your organization's backend; you can close Amy meanwhile, and the tile shows a red bubble when the report is ready.
   - **Mechanical checks**, without AI: the endpoints of each API definition against the routes in the code, both ways (a definition that matches none of the code's routes may describe another API and is only noted); ports a document names against `EXPOSE`, Compose ports, launch profiles, Kestrel URLs and `listen` calls; .NET, Node.js, Python, Java and Go versions against project files, `.nvmrc`, `engines`, `go.mod` and base images; environment variables and settings a document names that the code doesn't read, and, when the documents describe settings at all, those the code reads that no document mentions; base images; npm, `yarn`, `pnpm` and `dotnet run --project` commands against `package.json` scripts and project files.
   - **Check documents**: the AI takes the checkable statements out of each README and document (not decision records or change logs), Amy finds the code that names each one, and the AI says whether that code confirms or contradicts it, quoting the line. A statement nothing in the code names, or whose quoted line isn't found in the file, is listed as **could not be checked**, never as a conflict.
   - **Second opinion**: every contradiction the AI found is checked again with the model of the **Architecture Documentation** persona; only those it confirms stay conflicts.
   A service whose files and facts didn't change since its last check keeps that check's result without AI calls.
3. **Review.** The report: a summary per service (worst first), each service's conflicts with what the document says and what the code says (links to the checked commit), the statements that could not be checked, dismissed conflicts with their reasons, and a history with one row per check and how many conflicts were fixed since the last one. It goes below **`<project> Architecture Documentation`** when that exists, else to the top of the Wiki. A new report is shown as a preview (**Create the report page**); an earlier report is compared side by side and saved with the page's version. A page of that name that Amy didn't write is never changed. **Not a conflict** with a reason dismisses one for the whole team: the report lists it apart at once, and later checks count it out.

The way statements are checked comes from the **Check Repository Documentation** persona; administrators can give it a fast model (for example Haiku 5.5), since the second opinion uses the Architecture Documentation persona's model.

### Generate

Writes evidence-based documentation from the code into each repository's documentation folder (`docs` unless your administrators chose another) and opens a pull request per repository right away. There is no review step in Amy: the review happens on the pull request, by the teams or groups your administrators chose. Amy never pushes to the default branch; nothing reaches it unless a reviewer completes the pull request.

Findings are checked against the services a repository is connected to, so a repository isn't judged as if it stood alone. **A repository that already has documentation keeps it:** Amy takes up the code changes made since its documents' commit, checks its findings, and edits only what those change, in a small pull request, or in Amy's pull request that is still open. **Rewrite documentation that already exists** writes everything again, which costs as much as the first time.

**After a merge into the default branch** the same happens by itself, for repositories that already have documentation, unless administrators turned it off. A few minutes after the merge (the merges of a release make one update) a run by **Amy (automatic)** appears in the list. It reads only what changed: new endpoints get their sections, removed ones go, lines that moved are followed, findings the change fixes go, and new ones are added with their tech-debt records; the AI edits only the sections that mention what changed. The model comes from the administrators' rules, and the pull request says which and why. When the documents needn't change there is no pull request. A change Amy can't review publishes nothing, and the next merge starts from the same commit.

**Dev documents**, when administrators turned them on, say what the documentation on the default branch can't: how the dev branch differs, and what isn't released yet. Two documents in the dev branch's `dev-docs` folder (or another your administrators chose), by pull request into that branch: `main-dev-differences.md`, file by file, sorted into what is only on the dev branch, only on the default branch (hotfixes the dev branch doesn't have yet: the release would undo them) and changed on both, with endpoints that differ and findings whose lines are gone ("may be fixed"); and `unreleased.md`, the merged pull requests on the dev branch that aren't on the default branch, with their work items, reverts marked. They are written every two weeks unless your administrators chose otherwise; **What to write > Dev documents** on the Generate tab writes them now. Only files that changed since the last time are read, and the AI only says in words what differs: tests and changes of whitespace alone don't reach it, and it judges nothing. A release merge carries a snapshot to the default branch; the search index and Check Repository Documentation leave it out.

Everyone with the Code tab sees every run, who started it and what it found. Only the person who started a run or an administrator can stop, discard or retry it, or download its files; updates after merges and scheduled dev documents are the administrators'. While a run is going, nobody can start another in the same project or on the same repositories: Amy says whose run it is. An update after a merge holds only its repository, and waits for a run that holds it.

1. **Choose the repositories.** The list shows the repositories your administrators enabled that you can see, all checked (**Select all** and **Clear** help); a repository of another project shows its project. Until administrators enable some, the tab says so.
2. **Estimate** (without AI, also shown when you generate): per repository the files, the characters of code, the parts the AI reads one at a time, tokens and minutes, and the totals. It says how many files a run would leave out. A repository that is empty, disabled or can't be read by the backend says why, and one that has documentation says it will be corrected.
3. **Generate the documentation** asks once more, with the number of pull requests Amy will open; select **Start**. The job runs on your organization's backend, one repository at a time; you can close Amy meanwhile, and the tile shows a red bubble when it's done. First Amy maps which enabled repositories talk to each other, from patterns in their code and without AI; only what changed in them is read again. A repository without documentation then goes through six steps (one that has documentation follows the code and gets its findings checked, as above):
   - **Read**: the default branch, with the backend's own Azure DevOps access. All the code, however large its files are, as far as the backend's memory can hold it safely (what doesn't fit is listed under **Limitations**, tests first), with values that look like keys, passwords or connection strings masked; secrets files, dependencies, build output, binaries and lock files are skipped. Also README files, CLAUDE.md and the documentation folder. A repository needs no documentation yet.
   - **Discover**, without AI: the endpoints, framework by framework (Spring, ASP.NET controllers, minimal APIs and Razor Pages, Express, NestJS, Next.js, FastAPI, Flask, Go); ports per profile; framework and runtime versions; build and test commands; coverage thresholds; migrations, tests and the folder layout.
   - **Analyze**: the AI reads the code in parts. A quote of the code that isn't found in the file is left out, with what it supported. Parts whose files didn't change since the last run aren't read again.
   - **Write**: the documentation set (below), each document from the analysis and the code it cites. Before anything is written from them, findings that depend on something outside the repository are checked against the connected services: an agent reads those repositories' code on their default branches, only those enabled here and that the person who started the run may read. A finding comes out **Confirmed**, **Changed** (another severity), **Doesn't apply**, **Mitigated elsewhere**, **Needs a person** or **Not checked**. Anything but a confirmation or a question needs a line of code or of a document people wrote, which Amy finds in its file; documentation Amy generated proves nothing. A finding that doesn't apply stays listed with the reason but counts for nothing and gets no tech-debt record; a security finding is never dropped, at most mitigated. A check that can't be done never stops the documentation. High findings and bugs then get a second opinion from the model of the **Architecture Documentation** persona; those it doesn't confirm are marked **(verify)** with the reason.
   - **Check**: seven quality gates: every endpoint of the code documented and nothing else; no drift between the documents and the code (ports, versions, commands); finding and record IDs that resolve; relative links that resolve (a link to a file that doesn't exist becomes plain text); diagrams that render in Azure Repos; claims that need a person or an environment marked **(verify)**; and every document with its date and method. What fails is listed under **Limitations** and in the pull request.
   - **Pull request**: a branch `amy/docs/<date>-<commit>` with the documentation as the first commit and, unless administrators switched it off, the corrections to README files, CLAUDE.md and the folder's documents where the code contradicts them as the second, each an exact edit (what Amy can't place exactly is listed in the pull request to correct by hand). The description sums up the findings, the tech-debt priorities, the corrections and the gates.
4. **Results.** Per repository: its pull request (**updated** when Amy's earlier one was still open: the new commits are added to it), the findings by severity, the tech-debt records at P0, the claims to verify, the corrections, what the checks against the connected services found, and what its AI requests used (tokens, and about how much at list prices). **Up to date** means nothing needs doing: the documents say what the code does, the findings and the connected services are as they were when last checked, or, for a rewrite, the documentation on the default branch or in Amy's open pull request was written from this code with these settings. Findings checked before are asked about again only when a repository they were checked against moved, or when they couldn't be checked. A run doesn't stop for limits, outages or restarts of the backend: it waits and continues, and the repository's row says what it waits for. **Failed** with "Nothing was published" means a document couldn't be written: no incomplete documentation reaches the pull request, and Retry continues with what is done. A repository that fails doesn't stop the others. **Needs permission** means the backend's identity may not push or open the pull request: **Download .zip** gives you the written files, or an administrator grants the permission and you select **Retry 1 failed repository**. **New run** starts over; earlier runs stay in the list until they are discarded or the newest 20 of the project push them out.

**The documentation set**, in the documentation folder:

| File | Content |
| --- | --- |
| `README.md` | The index: each document with its counts, the method and the limitations |
| `REPOSITORY.md` | Purpose, technology stack with versions, package layout, the REST API as implemented, errors, conventions, configuration per profile, build, run and test |
| `architecture/architecture.md` | arc42 sections 1 to 12 with C4 and runtime diagrams (Mermaid) |
| `architecture/connected-services.md` | The services the code shows the repository calling, being called by, and exchanging events with, found without AI |
| `.amy/findings.json` | The findings as data, with what the checks found: what later runs read instead of the documents |
| `architecture/adr/NNNN-<title>.md` | Decision records the code shows |
| `api/README.md`, `api/<feature>.md` | Base URL, conventions and the error model; every endpoint in a numbered section with parameters, responses, edge cases and errors |
| `security/`, `reliability/`, `maintainability/`, `testing/`, `quality/`, `database/` | Six assessments, each finding with its ID, severity, the code it rests on, impact and recommendation. The database assessment only where there is a database; the security assessment as administrators set it |
| `tech-debt/README.md`, `tech-debt/TD-NN-<title>.md` | The tech-debt register: records with impact, likelihood and effort, priorities P0 to P3, a quadrant chart, options and a plan; `_TEMPLATE.md` for records the team adds |
| `.amy/manifest.json` | What Amy wrote, from which commit; findings, records and decision records keep their IDs from run to run |

Amy's text in each file sits between `<!-- amy:begin -->` and `<!-- amy:end -->`: what people write outside the markers is kept, and the next run rewrites only Amy's part. A document Amy no longer writes is deleted when it holds nothing else, otherwise marked "No longer generated".

How the documentation is written comes from the **Repository Documentation** persona (ground rules, style, how findings are rated, which standards apply). Generation uses the model set for it, or your own choice.

**Administrators** set it up under **Organization settings > Amy AI > Repository documentation**:

- **Projects** whose repositories get documentation, and the repositories of them to leave out. Enable only repositories that everyone who uses Amy may read: the documentation shows their code.
- **Reviewers of the pull requests**: search teams and groups and add up to 10. **Required reviewers** (on by default): a pull request can't be completed without them. The person who started the run is added as an optional reviewer.
- **Documentation folder** (`docs` by default), **Also correct what the code contradicts in README files, CLAUDE.md and the folder's documents** (on by default), **Open as drafts**, and **Security assessment**: **Full detail** (the default; shows the evidence of each finding to everyone who can read the repository), **Summary only** (findings without code or analysis) or **Left out**.
- **Keeping the documentation up to date**: updates after merges (on by default; they need the pull-request service hook, see [ADMIN-CONFIGURATION.md](ADMIN-CONFIGURATION.md)) and the model for a change: by how many code files it touches (tests aren't counted), a model for paths that hold certain words whatever the size, and the effort. A model not chosen is the default: Haiku up to 10 files, Sonnet up to 100, Opus for more and for the words `auth`, `security`, `login`, `token`, `crypto`, `migration`, `contract`, `openapi` and `swagger`, as far as the backend offers them. When a model's answers don't pass Amy's checks, the next one tries.
- **Dev documents** (off by default): the folder, the dev branches, the repositories with another one, how often and from when, the model, and their own reviewers. With no team or group nobody is asked to review, and each pull request completes by itself once the dev branch's policies pass.

The backend's own Azure DevOps identity reads the repositories and opens the pull requests: it needs Read, Contribute, Create branch and Contribute to pull requests on them ([PERMISSIONS.md](PERMISSIONS.md)).

## Diagnose a Ticket

For L1 support agents (Support tab). One shared session per ServiceNow ticket, with no limit on turns: anyone with Amy can open and continue it; only organization administrators can delete one.

1. Enter the **ServiceNow ticket number** (for example INC0012345) and select **Open**. A number with a session opens it where it stopped; **History** lists every session, newest first.
2. Paste the ticket's details and select **Send**. Amy first checks that it can find the problem quickly: the board (the part of the application, recognized by its keywords), who is affected, when it happened (to about an hour, with the time zone, or a request ID) and what the user did and saw. If something is missing, it says what, and writes a **Message for the user** to copy into the ticket. Every message for the user is written for someone who isn't technical, in the ticket's language, and is checked before you see it: one that names code, files, services, servers, logs, internal IDs or another person's details is rewritten, or left out with a note asking you to write it yourself. Paste the user's reply when it comes, or select **Diagnose anyway**: Amy then searches wider and marks the diagnosis as made with details missing. While she works, the card from Ask Amy shows what she searches and opens; **Stop** ends the turn and keeps your text in the box to send again.
3. With the details complete, Amy diagnoses: it reads the logs and traces around the reported time (when connected), the code that raises the errors, past bugs, pull requests and Wiki pages, and checks whether other users are affected. Every turn sees the whole conversation of the ticket. With Claude as your backend's AI provider, she works through her tools the same way as Ask Amy (files by their path, sources checked before the diagnosis).
4. The diagnosis is one of: **Resolve with the user** (numbered steps to send, such as clearing the cache and site data), **Configuration** (what an administrator must change), **Bug** (only with evidence) or **Route to L2** (with a handover note to paste into the ticket). For a bug, review and edit the drafted Bug (or switch it to a User Story), select **Check for duplicates**, then **Create Bug**, or add the ticket to a similar open work item as a comment. It is saved with your permissions in your organization's default location, with the ticket number in the description.

**Attached files.** With any turn, select **Attach files** (or paste a screenshot into the text box) to add up to 5 files: logs, exports, e-mails and screenshots, as in Ask Amy. **Send** works with files alone. Amy reads every file of the session in every later turn, and a time, e-mail address or request ID in a file counts as a detail of the ticket. The files are kept with the session (at most 100): select a file's name to open it as it was attached; **What Amy read in the screenshots** shows what Amy wrote down from each screenshot. Deleting the session deletes its files.

Personal data such as e-mail addresses in what you paste and attach is masked before it goes to the AI model; you see the real values. Screenshots are the exception: they go to the AI model as they are. The code, Wiki pages and work items Amy reads for the diagnosis go to the AI model with values that look like keys or passwords masked, but personal data in them (a customer's e-mail address in a work item, for example) as it is.

## Design Requirements

On the **Requirements** tab, where your organization's backend offers it. A chat in which Amy designs requirements with you on the Product Delight framework: what users need to get done (functional motivators) and how they want to feel (emotional motivators). The result is a requirements page tree in the Wiki, ready for Create Work Items. Designs are shared: everyone in the project with the Requirements tab sees them and can continue them.

The screen has three parts:

- **Designs** (left): **New design**, a search, **All** or **Mine**, and the project's designs, newest first, each with its last change, creator, number of planned requirements and **Draft** or **In the Wiki**.
- **The chat** (middle): describe what you want to build and for whom, and answer Amy's questions; where Amy offers answers, select one to put it in your message. **Attach files** (or paste a screenshot) adds up to 5 files per message, as in Ask Amy: research notes, interview transcripts, exports, screenshots. A line shows what Amy is doing. **Rename** changes the title Amy gave the design from your first message; its creator or an administrator can **Delete** it, after a second click (**Delete for everyone**).
- **Requirements** (right): what Amy keeps for the design, updated with every message: the stage, the balance, each requirement with its priority, delight type and slice, and the users and their motivators.

**How Amy works** (the method is the **Design Requirements** persona, which administrators can change):

1. **Discovery.** Amy splits the users into 2–4 segments whose motivations differ, writes their functional motivators as user goals and their emotional motivators as "I want to feel … when …", each tied to a functional one, and marks each as evidence (seen in data or research) or hypothesis. With little evidence, Amy drafts an interview guide that asks about feelings, not features.
2. **Ideation.** For the highest-stakes motivators, opportunities ("How might we help … while making them feel …?") and 3–6 ideas each, with three levers: remove friction, anticipate needs, exceed expectations. The best become planned requirements, with user stories and Given/When/Then acceptance criteria.
3. **Review.** Every deep and surface requirement is checked against seven delight traps (slows the task, wears off or annoys, wrong context, excludes users, breaks trust or privacy, over-promises, segment mismatch), with a fix for each failure; a validation plan for the top three deep requirements; and the top three recommendations: add, cut or upgrade.

Amy moves on when a stage is complete or you ask, and goes back when you want.

**What Amy computes** (the AI never decides these, so they are the same every time and explained on the pages):

- **Delight type**, from the motivators a requirement serves: functional and emotional is **deep**, functional only **low**, emotional only **surface**; without a motivator, **unknown**.
- **Priority:** **P0** for low delight the core job needs, on an important functional motivator (remove friction first); **P1** for deep delight on an important motivator, and other important friction; **P2** for deep delight on less important motivators, anticipating needs and other functional value; **P3** for surface delight, requirements without a motivator, and anything failing a delight trap until it's fixed. Say "make REQ-004 P1 because …" to set a priority yourself; your reason is recorded.
- **Balance** against 50% low, 40% deep, 10% surface (by effort when every requirement has one, else by count), with what it suggests. When surface delight goes over 10%, its least important requirements move to **Later**. The ratio is a guide: a mature or regulated product can lean low.
- **Slices:** MVP (P0 and P1), Next (P2 and P3), Later.

Nothing is forgotten in a long design: everything decided is kept in the requirements, which Amy sees in full with every message; older turns are summarized for Amy, and the whole conversation stays visible.

**Into the Wiki.** **Review and create in the Wiki** shows the page tree with a preview of each page: an overview, **Users and Motivators**, **Opportunities and Delight Grid**, **Requirements** (one page per feature), **Non-functional Requirements**, **Prioritization**, **Delight Traps**, **Risks, Assumptions and Open Questions** and **Ready for Development**. Choose the root page (`/Requirements/<title>` by default) and leave out pages you don't want; **Create N pages** creates them in the project's Wiki with your permissions. Design Requirements writes Wiki pages only; it creates no work items.

**Requirement Extensions.** Once the pages are in the Wiki, Amy never rewrites them. Later changes are listed under **Changes since the Wiki version**, each with **Revert to the Wiki version** (or **Keep the original wording** when only the wording changed). **Review and create Requirement Extension N** adds one page under the design's tree, `Requirement Extension N - <title>`: the goal, the changes at a glance, **What's new** (new requirements in full), **What's changing compared with the previous design** (before and after, field by field), **What's dropped**, **How it fits the existing design** and the open questions. Amy writes the title, goal and fit; the changes themselves are computed. The overview page gets a list of the extensions; what people wrote on it stays. Extensions are numbered 1, 2, 3 per design; while someone creates one, Amy holds its number for them, and anyone else trying at the same time is told who is creating it. Once a design is in the Wiki, Amy meets a new goal with the smallest change: more acceptance criteria before new requirements, extending a requirement before replacing it, and it says so when a change touches a planned requirement that work may have started on.

**Fit with the application.** When the project has run **How It Works Today**, Amy knows the application's screens, patterns and what each screen calls.
- Each requirement goes on an existing screen first (shown on its row and on a **Fit with the Application** page). It gets a new screen only with a reason, or no screen for what happens behind the scenes. Amy refuses screens that aren't in the map.
- Needs raised from different angles that land on the same screen are combined, and each requirement records who raised it and from which angle.
- **What this means technically** explains the change in plain words; questions only developers can answer go to **Questions for the developers**, never to you.
- Requirements that look alike are flagged.

**Redesign or extend.** A design can be new, a redesign or an extension. Amy describes how it works today (**What Exists Today**, with the problems people see) from How It Works Today and from what you add with **Add what exists**: Wiki pages with their subpages, work items by ID, or an earlier design. Each requirement says what it does to today's behaviour: keeps, changes, replaces or removes it (**What Changes**).

**One design per area.** After the first message of a new design, Amy checks the project's designs. When one is about the same area, it offers **Continue that design** (your message moves there) or **Start a separate design**. **Merge into…** moves a design's requirements into another with new IDs; the merged design stays to read. Amy then proposes combining what overlaps.

**Quality.** Every requirement is checked without the AI against INVEST, Given / When / Then and the qualities of ISO/IEC/IEEE 29148: acceptance criteria present and free of vague words (fast, easy, user-friendly…), a story that names who, what and why, one capability per requirement, measurable non-functional targets. **Check quality with Amy** adds Amy's review: ambiguity, missing error cases or permissions, inconsistencies, with a suggested rewrite. It also runs once when a review before the Wiki opens. Each requirement shows **Ready** or what's left to fix, and **Fix REQ-… with Amy** puts the request in the message box. **Ready for Development** lists what's left.

Personal data such as e-mail addresses is masked before anything goes to the AI model; you see the real values. Screenshots go to the AI model as they are. Designs and their files are kept until they are deleted.

## How It Works Today

Amy describes how the application works today, screen by screen, from its code, and writes it as a Wiki page tree in the Wiki of the project you run it in (`/Requirements/How It Works Today` by default).

1. Choose the repositories with the application's screens and its backend. Amy lists the screens it finds (the same way as Delight Check), with the app each belongs to; leave out screens or whole apps that don't matter. At most 400 screens per run. A screen keeps its number (SC1, SC2…) from run to run, so requirements placed on it still point to it. Optionally name Wiki pages to read as well, such as the architecture documentation.
2. **Describe the application** uploads each screen with the files it imports, and the backend files whose endpoints, schedules and events Amy reads without the AI. The run goes on on the backend; you can close the tab.
3. Review the pages: an overview with the areas and roles, one page per area (purpose, journeys, screens, rules) and one per screen. A screen page has **For everyone** (what it is for, what you can do, see and enter, the messages, where you go next) and **Technical notes** (route, file, what each action calls and where the backend handles it, the rules in the code). There are also **UI Patterns and Building Blocks**, **Behind the Scenes** and **Glossary** pages. Every field, action, message and rule comes with the line of code Amy quoted; what Amy can't quote is left out and counted.
4. **Create N pages** writes them with your permissions and saves Amy's copy of the map for the project.

Running it again compares with the pages as Amy last wrote them. New pages are created; changes to existing pages are shown side by side and placed on the page as people left it. A screen no longer in the code gets a note, not a deletion. When people correct the pages in the Wiki, **Take in edits people made in the Wiki** gives Amy their version. Design Requirements uses the saved map to fit requirements into the screens that exist.

## Delight Check

**Delight Check** (Requirements tab; when your administrator has turned it on) scores how much each page of your application delights its users, on the same Product Delight framework, from its code.

1. Choose the repositories that hold the application's UI. Amy lists the pages it finds:
   - **Angular apps** (any version) and **Flutter apps** from their routes: Amy reads `angular.json` or `pubspec.yaml`, then the route configuration (`RouterModule`, `provideRouter`, lazy modules and route files; GoRouter, `MaterialApp` routes), and adds the screens they open (Flutter's `Navigator.push`). Each page shows its app, for example "dispatch-web (Angular 21)".
   - Other frameworks by their folder conventions: Next.js, Remix, SvelteKit, Nuxt and Vue, React page and screen folders, Blazor, Razor Pages, ASP.NET MVC views, Thymeleaf templates.

   Leave out the pages or whole apps that don't matter, and mark **key pages**: they count double in the overall score. At most 400 pages per check.
2. Choose where the users' **motivators** come from: a Design Requirements design, or none, and Amy infers each page's likely motivators and marks them to verify with users.
3. Select **Start check**. Amy reads each page's file and the files it uses (two levels deep, up to 40,000 characters per page; an Angular component's template counts as part of the component, and its child components are found by their selectors), masks values that look like secrets, and sends them to your organization's AI backend. You can close Amy; the tile shows a red bubble when the check is done.

Each page gets a score from 0 to 100: **low delight** (a task that flows, feedback and status, error prevention and recovery, empty states, speed, accessibility, clear words: 50 points), **deep delight** (anticipating needs, easing stressful moments, confidence and control, personal connection: 40), **surface delight** (enhancers used with restraint, reduced motion, no distraction: 10), less 5 points per **delight killer** (dark patterns, interrupting dialogs, risk of losing data, inaccessible critical paths, misleading wording; at most 20). Amy checks every quote of the code the AI gives; a criterion it can't find, or the code can't show (timing, visuals), is left out of the score, and the **confidence** says how much could be assessed. Each page lists its recommendations with the code to change, the points each gains and the **best reachable** score.

The result is one Wiki page, **Delight Check** (below **/Requirements** when your Wiki has one), shown for your review before it is saved; a later check is compared side by side, with each page's previous score and a history. From the result, **Design requirements from these recommendations** starts a design with them, and **Create work items** opens Create Work Items with them. Pages unchanged since the last check reuse their assessment.

## Mockup Designer

**Mockup Designer** (Requirements tab; when your administrator has turned it on and added your apps under Organization settings > Amy AI > Mockup Designer, and the backend's AI provider is Claude) makes clickable mockups of your apps' screens in a chat: show the screen, say what should change, look at the result, and keep refining it. Amy's designer works the way a person with a browser would.

1. Select **New mockup**, then show Amy the screen: paste or add **screenshots** (up to 20 per message, 100 per mockup), or paste a **link** to the page into the message, and say what you want, for example "Add a status filter above the list".
2. Amy reads the screen's code (its texts, what each control does and where it leads, its other states), the app's theme, fonts, icons and images, and writes the mockup as one page with its own scripts, so filters, pop-ups, toggles, tabs and dark mode really work. A screenshot is the truth for the look: Amy measures it (its exact colors, edges, texts and icons) and copies it with made-up data, never the names and numbers in your screenshot. When your administrator has given the app a test environment and the backend has a browser for it (Amy 0.35.0), Amy opens the page there herself, clicks and types her way to the state you describe, and starts from a copy of the page as that browser drew it, then makes your change in it. A Flutter app can't be copied, since it draws on a canvas. With its web build as the test environment (Amy 0.36.0), Amy goes through its screens by their buttons and fields and builds the mockup from exact screenshots and positions of the real screens.
3. Then she checks her work in your browser: she looks at the page at several widths, compares it with your screenshots pixel by pixel, tries it with scripts (opens things, types, switches the theme) and scans it for accessibility, and fixes what she finds until it matches and works. What she is doing shows below the conversation, in the card from Ask Amy with the phases Understand, Build, Look and Polish; keep the panel open while she works, since the looking happens in your browser. **Stop** ends the message without a new version: the mockup stays as it was, and your message stays in the box.
4. Each message makes a **version**: Amy reports what she built, what she assumed and what she couldn't check, the preview shows the version on the right, and **Open in a new tab** opens it as a page of its own. Every message builds on the last version. A flow of several screens is one page whose buttons and links lead from screen to screen, and the browser's back button works.

- **From a design.** The Mockup Designer lists the project's Design Requirements designs under **Designs**, at any stage; **Mock up** starts a mockup of one (in Design Requirements, **Mock up this design** does the same). The brief carries the design's problem, how it works today, its motivators and every requirement not dropped, ideas marked as ideas, with the screens How It Works Today names for them.
- **Wiki and web.** A new mockup's first message also brings the project's Wiki pages it may concern, read with your own permissions; with web search on in your Amy settings, Amy may search the web for public facts.
- **Model and effort** follow the *Mockup Designer* persona (high effort recommended); Amy's answers aren't capped.
- **Links.** Mockups are shared with everyone in the project. Each mockup has a link that anyone who has it can open without signing in, for as long as your administrator set; untick **Anyone with the link can open the versions** to stop it. The pages run their own scripts in a sandbox with no network: they load nothing from elsewhere, send nothing and open nothing.
- **Play scenario.** A mockup with more than one screen, state or interface comes with a scenario: **Play scenario**, in a bar at the bottom of the mockup (in the preview and behind a shared link), walks through the process step by step, with a caption for each step and the control it uses marked. While it plays, nothing else on the mockup responds to clicks or the keyboard: **Pause** holds it and **Play** continues, and **Reset demo** takes the mockup back to step one, where you can click through it yourself or play it again. If you clicked around first, **Play scenario** still starts from step one. Amy plays each scenario once in your browser before publishing it. For a mockup without one, ask Amy in the chat: "add a play scenario".
- **Earlier mockups.** Mockups made before 0.28.0 keep their versions; a new message continues them with Amy's designer.

Amy only reads the apps' repositories and storage containers: nothing in them changes.

## Create Sprint Schedule

1. Choose the area in **Board (team)**, the **Generation period** (6, 12 or 24 months) and the **Sprint duration** in whole weeks. Sprints are named after the area (its last segment) and added to the board of the team whose default area is the selected area or its closest parent; the team is shown under the list.
2. Check the **First sprint start date** (default: the day after the area's last current or future sprint ends, or the team's when the area has no sprints of its own) and the **Starting sprint number** (default: the next number when every existing sprint name has one; 1 for an area without sprints). An area's sprints are all the team's sprints when the team's default area is that area, otherwise the team's sprints named after the area. When Amy cannot tell, these fields are empty and must be filled.
3. Choose the naming convention, `<area> <sprint number> <sprint start date> - <sprint end date>` (for example `DevOps 14 28.03.27 - 11.04.27`, dates as DD.MM.YY) or `<area> <sprint number>`. The example name updates as you type.
4. Select **Preview schedule**. Every sprint is listed with number, name, start and end date. End dates are inclusive: a two-week sprint starting on a Monday ends on the Sunday 13 days later. Sprints keep their full length; one that ends after the period is marked **Extends**.
5. Conflicts (a name that already exists, dates overlapping one of the area's sprints) are resolved in one click, or by changing the settings; existing sprints are never changed. A sprint whose dates are taken has **Leave out** (the existing sprint covers those dates; **Put back** undoes it), and the sprints kept are numbered on without gaps. A taken name has **Start at N**, the first starting number whose names are free. **Resolve all** does both.
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
