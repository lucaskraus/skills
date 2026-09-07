---
name: create-ticket
description: Write and create a ticket in a team management platform (Jira, Linear, ClickUp, Notion or any other connected via MCP). Use when the user asks to create, write, or open a ticket, task, issue, or story, or wants work broken into tickets.
---

# Create Ticket

Write a ticket a developer can pick up and implement without asking questions, then create it through the platform's MCP server.

Hard rules for every ticket:

- Objective. Each section is a few sentences or a short list. Every line must carry information the implementer needs.
- No em dashes anywhere in the ticket, the title, or your replies. Use commas, periods, or parentheses.

## Steps

### 1. Pick the platform

1. If the prompt names the platform, use it.
2. Otherwise, list the connected MCP servers that manage tickets (Jira via Atlassian, Linear, ClickUp, Notion, and similar) and ask the user which one to use. When only one is connected, confirm it in the same question.

**Done when:** the user has confirmed one platform and its MCP tools are loaded.

### 2. Gather context

1. Read the parent Epic or Story when the prompt links one (fetch it through the platform's MCP). It defines the full valid state the ticket contributes to.
2. Explore the repository: the folders, patterns, and existing files the task will touch (request layer, similar endpoints, sibling components, schemas, tests). The suggested solution must cite real paths and conventions, not generic advice.
3. When the task touches an external resource (Stripe, a third-party API, a cloud service), locate the official documentation page for the exact feature.
4. Ask the user for anything still missing that the ticket cannot be written without (project, issue type, parent, missing business rule).

**Done when:** you can name the concrete files, endpoints, or components the work involves and the parent ticket's scope is known.

### 3. Write the description

Use exactly these four sections, in this order. The color in parentheses is the section heading color on platforms that support colored text (Jira does, check the platform's content format guide). On platforms without colors, use plain headings.

**Summary (yellow)**
Business language. What should be done, what problem it solves, and what improves once it ships. State the business rules that apply. No technical terms.

**Suggested solution (light blue)**
Technical language, grounded in the repository. Name the files to create or change, the endpoint and its contract, validation, and for frontend work the props contract of the screen or component and the schemas involved. It is a suggestion, so keep it to the shape of the solution, not a full implementation.

**Valid if... (orange)**
The acceptance criteria as a checklist. Each item is a condition someone can verify: follows the repository pattern for X, lists all Y by page and pageSize, unit tests cover the new components, the user can filter by Z. Derive the full list from the prompt and the parent Epic or Story.

**Additional information (purple)**
Documentation links for every external resource the task touches, plus any related tickets. Omit the section when there is nothing to add.

Title: `[PREFIX] - [summary]`. The prefix is the knowledge field: `BE` for backend, `FE` for frontend, `DEVOPS` for infrastructure and pipelines. Use a different prefix only when the user names one in the prompt, or when the task clearly fits none of the three (rare); in that case propose a prefix and confirm it in step 4. The summary is short, imperative, and names the deliverable, e.g. "[FE] - Add transactions list with pagination and filters".

**Done when:** every section is filled or deliberately omitted, every item in Valid if is verifiable, every path in Suggested solution exists in the repository or is clearly marked as new, and the text contains no em dashes.

### 4. Confirm with the user

Show the title and full description. Apply the requested edits before creating.

**Done when:** the user approves the draft.

### 5. Create the ticket

1. Fetch the platform's content format guide when it offers one, then convert the description to that format, applying the section colors where supported.
2. Create the ticket in the confirmed project with the confirmed issue type and parent.
3. Report the ticket key and URL.

**Done when:** the ticket URL is shown to the user.
