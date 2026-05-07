---
name: mintlify
description: Comprehensive reference for building Mintlify documentation sites. Use when creating pages, configuring docs.json, adding components, setting up navigation, or working with API references. Routes to detailed reference files for all components and configuration options.
---

# Mintlify reference

Reference for building documentation with Mintlify. This file covers essentials that apply to every task. For detailed reference on specific topics, read the files listed in the reference index below.

## Reference index

Read these files **only when your task requires them**. They are in the `reference/` directory next to this file.

| File | When to read |
|------|-------------|
| `reference/components.md` | Adding or modifying components (callouts, cards, steps, tabs, accordions, code groups, fields, frames, icons, tooltips, badges, trees, mermaid, panels, prompts, colors, tiles, updates, views). |
| `reference/configuration.md` | Changing docs.json settings (theme, colors, logo, fonts, appearance, navbar, footer, banner, redirects, SEO, integrations, API config). Also covers snippets, hidden pages, .mintignore, custom CSS/JS, and the complete frontmatter fields table. |
| `reference/navigation.md` | Modifying site navigation structure (groups, tabs, anchors, dropdowns, products, versions, languages, OpenAPI in nav). |
| `reference/api-docs.md` | Setting up API documentation (OpenAPI, AsyncAPI, MDX manual API pages, extensions, playground config). |

## MCP servers

Two Mintlify MCP servers are available. Use them alongside the reference files in this skill.

### Mintlify (docs MCP)

Read-only access to Mintlify's published documentation. Use it when the reference files don't cover a specific detail, when you need an up-to-date component signature, or to verify an unfamiliar config option.

Tools:
- `search_mintlify` — Search the Mintlify knowledge base by query. Good for finding guides, examples, and API references.
- `query_docs_filesystem_mintlify` — Browse the docs file tree (`ls`, `cat`, `grep`, `find`, etc.). Good for reading a specific docs page.

### Mintlify MCP (dashboard MCP)

Write access to a Mintlify project. Requires OAuth login on first use — Claude Code will open a browser window to authenticate.

Use this server when the user wants to edit their Mintlify content, restructure navigation, or open a pull request. All changes happen on a branch and must be reviewed before merging.

Workflow: call `checkout` first (always), then use `read`/`search`/`edit_page`/`write_page`/`list_nodes`/`create_node`/`update_node`/`move_node`/`delete_node`/`update_config` to make changes, then call `save` to open a PR (or `discard_session` to abandon).

Key tools:
- **`checkout`** — Start a session on a branch (required first call). Returns an `editorUrl` to preview changes live.
- **`list_branches`** — List existing branches; call before `checkout` to attach to one.
- **`read`** / **`search`** — Fetch a page's MDX or search across pages.
- **`edit_page`** / **`write_page`** — Apply targeted edits or overwrite a page.
- **`list_nodes`** / **`create_node`** / **`update_node`** / **`move_node`** / **`delete_node`** — Manage the navigation tree.
- **`update_config`** — Modify `docs.json` (theme, nav roots, integrations, SEO).
- **`diff`** — See all changes relative to `main`.
- **`save`** — Open a PR (`mode: "pr"`) or push to the branch (`mode: "commit"`).
- **`discard_session`** — Drop all in-session changes.

<Note>
Keep each session focused on one change. Smaller sessions produce easier-to-review PRs. Open the `editorUrl` to watch changes render live.
</Note>

## Before you start

Read the project's `docs.json` file first. It defines the site's navigation, theme, colors, and configuration.

Search for existing content before creating new pages. You may need to update an existing page, add a section, or link to existing content rather than duplicating.

Read 2-3 similar pages to match the site's voice, structure, and formatting.

## File format

Mintlify uses MDX files (`.mdx` or `.md`) with YAML frontmatter.

```
project/
├── docs.json           # Site configuration (required)
├── index.mdx
├── quickstart.mdx
├── guides/
│   └── example.mdx
├── openapi.yml         # API specification (optional)
├── images/             # Static assets
│   └── example.png
└── snippets/           # Reusable components
    └── component.jsx
```

### File naming

- Match existing patterns in the directory
- If no existing files or mixed file naming patterns, use kebab-case: `getting-started.mdx`
- Add new pages to `docs.json` navigation or they won't appear in the sidebar

### Internal links

- Use root-relative paths without file extensions: `/getting-started/quickstart`
- Do not use relative paths (`../`) or absolute URLs for internal pages

### Images

Store images in an `images/` directory. Reference with root-relative paths. All images require descriptive alt text.

```mdx
![Dashboard showing analytics overview](/images/dashboard.png)
```

## Page frontmatter

Every page requires `title` in its frontmatter. Include `description` and `keywords` for SEO.

```yaml
---
title: "Clear, descriptive title"
description: "Concise summary for SEO and navigation."
keywords: ["relevant", "search", "terms"]
---
```

### Common frontmatter fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | Page title in navigation and browser tabs. |
| `description` | string | No | Brief description for SEO. Displays under the title. |
| `sidebarTitle` | string | No | Short title for sidebar navigation. |
| `icon` | string | No | Lucide, Font Awesome, or Tabler icon name. Also accepts a URL or file path. |
| `tag` | string | No | Label next to page title in sidebar (e.g., "NEW"). |
| `hidden` | boolean | No | Remove from sidebar. Page still accessible by URL. |
| `mode` | string | No | Page layout: `default`, `wide`, `custom`, `frame`, `center`. |
| `keywords` | array | No | Search terms for internal search and SEO. |
| `api` | string | No | API endpoint for interactive playground (e.g., `"POST /users"`). |
| `openapi` | string | No | OpenAPI endpoint reference (e.g., `"GET /endpoint"`). |

## Quick component reference

Below are the most commonly used components. For full props and all 24 components, read `reference/components.md`.

### Callouts

```mdx
<Note>Supplementary information, safe to skip.</Note>
<Info>Helpful context such as permissions or prerequisites.</Info>
<Tip>Recommendations or best practices.</Tip>
<Warning>Potentially destructive actions or important caveats.</Warning>
<Check>Success confirmation or completed status.</Check>
<Danger>Critical warnings about data loss or breaking changes.</Danger>
```

### Steps

```mdx
<Steps>
  <Step title="First step">
    Instructions for step one.
  </Step>
  <Step title="Second step">
    Instructions for step two.
  </Step>
</Steps>
```

### Tabs and code groups

```mdx
<Tabs>
  <Tab title="npm">
    ```bash
    npm install package-name
    ```
  </Tab>
  <Tab title="yarn">
    ```bash
    yarn add package-name
    ```
  </Tab>
</Tabs>
```

```mdx
<CodeGroup>

```javascript example.js
const greeting = "Hello, world!";
```

```python example.py
greeting = "Hello, world!"
```

</CodeGroup>
```

### Cards and columns

```mdx
<Columns cols={2}>
  <Card title="First card" icon="rocket" href="/quickstart">
    Card description text.
  </Card>
  <Card title="Second card" icon="book" href="/guides">
    Card description text.
  </Card>
</Columns>
```

Use `<Columns>` to arrange cards (or other content) in a grid. `cols` accepts 1-4.

### Accordions

```mdx
<AccordionGroup>
  <Accordion title="First section">Content one.</Accordion>
  <Accordion title="Second section">Content two.</Accordion>
</AccordionGroup>
```

## CLI commands

Install the CLI with `npm i -g mint`.

### Local development

- `mint dev` — Start local preview at localhost:3000. `--port` sets the port. `--no-open` skips browser launch. `--groups <names>` mocks user groups. `--disable-openapi` skips OpenAPI processing. `--local-schema` allows locally-hosted OpenAPI files over HTTP.
- `mint validate` — Strict build validation; exits non-zero on warnings or errors. `--groups <names>` mocks user groups. `--disable-openapi` skips OpenAPI processing. `--local-schema` allows local OpenAPI files.
- `mint export` — Export a static site zip for air-gapped deployment. `--output <file>` sets the output path (default: `export.zip`). `--groups <names>` includes restricted pages. `--disable-openapi` skips OpenAPI processing.

### Content quality

- `mint broken-links` — Check for broken internal links. `--check-anchors` validates `#` anchors. `--check-external` checks external URLs. `--check-redirects` checks that redirect destinations in `docs.json` resolve. `--check-snippets` checks links inside `<Snippet>` components.
- `mint a11y` — Accessibility checks (alt text, color contrast). `--skip-contrast` or `--skip-alt-text` to narrow scope.
- `mint score [url]` — Score a docs site's AI/agent readiness. Checks llms.txt, MCP discoverability, robots.txt, sitemap, structured data, response latency, and more. Requires `mint login`. Defaults to your configured subdomain. `--format` accepts `table` (default), `plain`, or `json`.

### Analytics

- `mint analytics stats` — KPI numbers (views, visitors, searches, feedback, assistant usage). Options: `--subdomain`, `--from`, `--to`, `--format` (table/plain/json/graph), `--agents`/`--humans` to filter traffic, `--page` to filter to one path.
- `mint analytics search` — Search analytics. `--query` filters by search term substring. `--page` filters by top clicked page.
- `mint analytics feedback` — Feedback analytics. `--type`: `page` (aggregate by page) or `code` (code snippet feedback). `--page` filters to one path.
- `mint analytics conversation list` — List assistant conversations. `--page` filters by page referenced in sources.
- `mint analytics conversation view <id>` — View a single conversation.
- `mint analytics conversation buckets list` — List conversation category buckets.
- `mint analytics conversation buckets view <id>` — View conversations in a bucket.

### Authentication

- `mint login` — Authenticate your Mintlify account.
- `mint logout` — Log out of your account.
- `mint status` — Show current authentication status (CLI version, email, org, subdomain).

### Configuration

- `mint config set <key> <value>` — Persist a config value. Valid keys: `subdomain`, `dateFrom`, `dateTo`.
- `mint config get <key>` — Read a stored config value.
- `mint config clear <key>` — Remove a stored config value.

### Project setup

- `mint new [directory]` — Scaffold a new Mintlify docs site. `--name` and `--theme` set initial config. `--template` selects a pre-defined template. `--force` overwrites an existing directory.
- `mint workflow` — Add a workflow to the docs repository.

### Maintenance

- `mint update` — Update the CLI to the latest version.
- `mint version` — Show installed CLI and client versions.

### Coming soon

These commands are registered but not yet functional. Running them records interest via telemetry.

- `mint ai` — AI-powered documentation tools.
- `mint test` — Documentation testing.
- `mint signup` — Account sign-up from the CLI.
- `mint mcp` — MCP server for documentation.

## Writing standards

- Second-person voice ("you").
- Active voice, direct language.
- Sentence case for headings ("Getting started", not "Getting Started").
- Sentence case for code block titles.
- All code blocks must have language tags.
- All images must have descriptive alt text.
- No marketing language, filler phrases, or emoji.
- Keep code examples simple, practical, and tested.

## Common mistakes

- Missing language tag on a code block (use ` ```python `, not ` ``` `).
- Using relative paths (`../page`) instead of root-relative (`/section/page`).
- Forgetting to add new pages to `docs.json` navigation.
- Images without alt text.
- Adding file extensions to internal links (`/page.mdx` instead of `/page`).
