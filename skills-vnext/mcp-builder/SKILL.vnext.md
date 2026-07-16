---
name: mcp-builder
description: Guide for creating high-quality MCP (Model Context Protocol) servers that enable LLMs to interact with external services through well-designed tools. Use when building MCP servers to integrate external APIs or services, whether in Python (FastMCP) or Node/TypeScript (MCP SDK).
license: Complete terms in LICENSE.txt
---

# MCP Server Development Guide

## Overview

Create MCP servers that enable LLMs to interact with external services. The only
quality measure that matters: **can an LLM that has never seen your code complete a
real task with these tools, in few calls, without flooding its context?** Every
decision below — naming, descriptions, response shape, errors — is downstream of
that test. Judge each choice by imagining the model mid-task: what does it know,
what is it guessing at, what is it wading through?

# Process

## Phase 1: Research and Planning

### 1.1 Design judgment (learned the hard way)

**API coverage vs. workflow tools.** Comprehensive coverage gives agents flexibility
to compose operations; workflow tools bundle a multi-step job into one call. When
uncertain, prioritize comprehensive coverage — a missing primitive blocks an agent
completely, while a missing convenience just costs extra calls. Add a workflow tool
only when a real task takes 4+ chained calls with no decision points between them
(e.g. "create release: tag + notes + publish"). A workflow tool with a judgment call
in the middle is a trap: the agent can't intervene halfway.

**Naming is discoverability.** Agents pick tools by name first, description second.
Use a consistent `service_verb_noun` scheme (`github_create_issue`,
`github_list_repos`). If two tools' names don't tell an agent which one to pick,
rename until they do.

**Context is the scarce resource.** Every byte a tool returns is prompt text the
agent pays for on every subsequent turn. Default list operations to compact fields +
pagination; provide a `get` tool for full detail. Returning everything "to be safe"
is the most common MCP defect.

**Errors are instructions.** An agent that hits an error will retry; your error
message decides whether the retry is smarter. Every error should say what went
wrong, why (if known), and what to try instead.

### 1.2 Study the MCP protocol

Start with the sitemap: `https://modelcontextprotocol.io/sitemap.xml`, then fetch
specific pages with the `.md` suffix (e.g.
`https://modelcontextprotocol.io/specification/draft.md`). Review the spec overview,
transports (streamable HTTP, stdio), and tool/resource/prompt definitions.

### 1.3 Study framework documentation

**Recommended stack:** TypeScript (high-quality SDK, strong typing, models generate
it well) with streamable HTTP + stateless JSON for remote servers, stdio for local.

Load as needed:
- [📋 MCP Best Practices](./reference/mcp_best_practices.md) — core guidelines
- TypeScript SDK: `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
  and [⚡ TypeScript Guide](./reference/node_mcp_server.md)
- Python SDK: `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
  and [🐍 Python Guide](./reference/python_mcp_server.md)

### 1.4 Plan the implementation

Review the service's API docs for endpoints, auth, and data models (WebFetch/search
as needed). List the endpoints to implement, most common operations first. For each,
decide the response shape *now* — what an agent needs, not what the API returns.

## Phase 2: Implementation

### 2.1 Project structure

See the language guides for setup: [⚡ TypeScript](./reference/node_mcp_server.md) /
[🐍 Python](./reference/python_mcp_server.md).

### 2.2 Core infrastructure

Shared utilities first: API client with auth, error-handling helpers, response
formatting, pagination support.

### 2.3 Implement tools — with the quality bar shown

**Tool descriptions.** The description is the tool's UI. Worked example:

Bad — restates the name, answers none of the agent's questions:
> `search_issues`: Searches for issues.

Good — says what it covers, what comes back, and when to prefer a sibling tool:
> `jira_search_issues`: Search issues with JQL or plain text. Returns up to
> `limit` (default 20, max 100) issues with key, summary, status, assignee, and
> updated date — use `jira_get_issue` for full descriptions and comments. Prefer
> this over `jira_list_issues` when filtering by text, status, or assignee.

**Input schemas.** Zod (TypeScript) or Pydantic (Python). Constraints and one
concrete example per non-obvious field (`project: "PLAT" — the project key, not its
display name`). An agent that has to guess a format will guess wrong once per
session.

**Response shaping.** Worked example — a `list_issues` call against an API returning
2KB per issue:

Bad: return the raw API array. 20 issues ≈ 40KB of mostly-unused JSON in context.

Good: return per issue only `key, summary, status, assignee, updated` plus
`{"total": 512, "next_cursor": "..."}`, and a one-line hint when truncated
("512 matches; showing 20. Refine the query or pass cursor."). Structure via
`outputSchema`/`structuredContent` where the SDK supports it; use Markdown for
human-oriented prose, JSON for data the agent will filter.

**Error messages.** Worked example:

Bad: `Error: 404`

Good: `Issue PLAT-9999 not found in project PLAT (highest existing: PLAT-812).
Check the key, or use jira_search_issues to find the issue by title.`

The good version converts a dead end into a next move. Apply the same standard to
auth failures (say which credential/scope is missing), rate limits (say when to
retry), and validation errors (name the field and the expected format).

**Annotations.** Set `readOnlyHint`, `destructiveHint`, `idempotentHint`,
`openWorldHint` honestly — clients use them to decide what needs confirmation.

## Phase 3: Review and Test

- Code quality: DRY, consistent error handling, full type coverage.
- **TypeScript:** `npm run build`, then MCP Inspector
  (`npx @modelcontextprotocol/inspector`). **Python:** `python -m py_compile`, then
  Inspector.
- The test that matters: pick 3 realistic tasks and walk them as the agent would,
  reading only tool names/descriptions. Every point where *you* had to peek at the
  source is a defect in the descriptions.

See the language guides for detailed testing and quality checklists.

## Phase 4: Create Evaluations

Load the [✅ Evaluation Guide](./reference/evaluation.md) for full guidelines.

Create 10 questions that test whether an LLM can use the server for realistic,
complex work: inspect the tools, explore real data with read-only calls, write the
questions, then **solve each one yourself to verify the answer**. Each question must
be independent, read-only, complex (multiple tool calls), realistic, verifiable by
string comparison, and stable over time.

Output format:

```xml
<evaluation>
  <qa_pair>
    <question>Find discussions about AI model launches with animal codenames. One model needed a specific safety designation that uses the format ASL-X. What number X was being determined for the model named after a spotted wild cat?</question>
    <answer>3</answer>
  </qa_pair>
<!-- More qa_pairs... -->
</evaluation>
```

# Reference Files

Load as needed during development:

- **MCP Protocol**: sitemap at `https://modelcontextprotocol.io/sitemap.xml`, pages via `.md` suffix
- [📋 MCP Best Practices](./reference/mcp_best_practices.md) — naming, response formats, pagination, transport selection, security
- [⚡ TypeScript Guide](./reference/node_mcp_server.md) — structure, Zod patterns, `server.registerTool`, working examples, checklist
- [🐍 Python Guide](./reference/python_mcp_server.md) — FastMCP patterns, Pydantic models, `@mcp.tool`, working examples, checklist
- [✅ Evaluation Guide](./reference/evaluation.md) — question creation, verification, XML format, runner scripts
