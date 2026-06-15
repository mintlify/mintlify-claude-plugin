# CLI reference

Full reference for every `mint` CLI command and flag.

Install with `npm i -g mint`.

## Global flags

Available on all commands.

| Flag | Description |
|------|-------------|
| `--telemetry` | Enable or disable anonymous usage telemetry. |
| `--help`, `-h` | Display help for the command. |
| `--version`, `-v` | Display the CLI version. Alias for `mint version`. |

## Local development

- `mint dev` — Start local preview at localhost:3000. `--port` sets the port. `--no-open` skips browser launch. `--groups <names>` mocks user groups. `--disable-openapi` skips OpenAPI processing. `--local-schema` allows locally-hosted OpenAPI files over HTTP.
- `mint validate` — Strict build validation; exits non-zero on warnings or errors. `--groups <names>` mocks user groups. `--disable-openapi` skips OpenAPI processing. `--local-schema` allows local OpenAPI files.
- `mint export` — Export a static site zip for air-gapped deployment. `--output <file>` sets the output path (default: `export.zip`). `--groups <names>` includes restricted pages. `--disable-openapi` skips OpenAPI processing.

## Content quality

- `mint broken-links` — Check for broken internal links. `--check-anchors` validates `#` anchors. `--check-external` checks external URLs. `--check-redirects` checks that redirect destinations in `docs.json` resolve. `--check-snippets` checks links inside `<Snippet>` components.
- `mint a11y` — Accessibility checks (alt text, color contrast). `--skip-contrast` or `--skip-alt-text` to narrow scope.
- `mint score [url]` — Score a docs site's AI/agent readiness. Checks llms.txt, MCP discoverability, robots.txt, sitemap, structured data, response latency, and more. Requires `mint login`. Defaults to your configured subdomain. `--format` accepts `table` (default), `plain`, or `json`.

## Analytics

All `mint analytics` subcommands share these flags: `--subdomain`, `--from`, `--to`, `--format` (plain/table/json/graph; default: plain).

- `mint analytics stats` — KPI numbers (views, visitors, searches, feedback, assistant usage). `--page` filters to one path.
- `mint analytics search` — Search analytics. `--query` filters by search term substring. `--page` filters by top clicked page.
- `mint analytics feedback` — Feedback analytics. `--type`: `page` (aggregate by page) or `code` (code snippet feedback). `--page` filters to one path.
- `mint analytics conversation list` — List assistant conversations. `--page` filters by page referenced in sources.
- `mint analytics conversation view <id>` — View a single conversation.
- `mint analytics conversation buckets list` — List conversation category buckets.
- `mint analytics conversation buckets view <id>` — View conversations in a bucket.

## Authentication

- `mint login` — Authenticate your Mintlify account.
- `mint logout` — Log out of your account.
- `mint status` — Show current authentication status (CLI version, email, org, subdomain).

## Configuration

- `mint config set <key> <value>` — Persist a config value. Valid keys: `subdomain`, `dateFrom`, `dateTo`.
- `mint config get <key>` — Read a stored config value.
- `mint config clear <key>` — Remove a stored config value.

## Project setup

- `mint new [directory]` — Scaffold a new Mintlify docs site. `--name` and `--theme` set initial config. `--template` selects a pre-defined template. `--force` overwrites an existing directory.

## Workflows

All `mint workflow` subcommands share these flags: `--subdomain`, `--format` (table/json; default: table).

- `mint workflow create` — Create a workflow. Requires exactly one trigger: `--cron <expr>` for scheduled or `--push-repo <owner/repo>` (repeatable) for push-triggered. Key flags: `--name`, `--type` (one of `changelog`, `source-code-agent`, `translations`, `writing-style`, `typo-check`, `broken-link-detection`, `seo-metadata-audit`, `assistant-docs-updates`, `contextual-feedback-docs-updates`; omit for custom), `--prompt`, `--context-repo` (repeatable, up to 10), `--automerge`, `--file <path>` (JSON/YAML file overrides inline flags).
- `mint workflow list` — List workflows for the current deployment.
- `mint workflow delete <id>` — Delete a workflow by ID. Use `mint workflow list` to get the ID.

## Maintenance

- `mint update` — Update the CLI to the latest version.
- `mint version` — Show installed CLI and client versions.

## Coming soon

These commands are registered but not yet functional. Running them records interest via telemetry.

- `mint ai` — AI-powered documentation tools.
- `mint test` — Documentation testing.
- `mint signup` — Account sign-up from the CLI.
- `mint mcp` — MCP server for documentation.
