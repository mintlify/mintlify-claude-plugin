# API documentation reference

Setting up API documentation with OpenAPI, AsyncAPI, and MDX manual pages.

## OpenAPI setup

Add your OpenAPI spec to `docs.json`:

```json
"api": {
  "openapi": "openapi.json"
}
```

Multiple specs:

```json
"api": {
  "openapi": ["openapi/v1.json", "openapi/v2.json"]
}
```

Reference individual endpoints in navigation:

```json
{
  "group": "Users",
  "openapi": "openapi.json",
  "pages": ["GET /users", "POST /users", "GET /users/{id}"]
}
```

## OpenAPI extensions

- `x-hidden`: Creates page but hides from navigation.
- `x-excluded`: Completely excludes endpoint from docs.
- `x-codeSamples`: Custom code examples per endpoint.

```yaml
paths:
  /users:
    get:
      x-codeSamples:
        - lang: "bash"
          label: "List users"
          source: |
            curl https://api.example.com/users
```

## MDX manual API pages

For endpoints without an OpenAPI spec:

```yaml
---
title: "Create user"
api: "POST https://api.example.com/users"
---
```

Or with a base URL configured in `docs.json`:

```yaml
---
title: "Create user"
api: "POST /users"
---
```

## AsyncAPI

For WebSocket and event-driven APIs:

```json
"api": {
  "asyncapi": "asyncapi.yaml"
}
```

Reference channels in frontmatter:

```yaml
---
title: "WebSocket channel"
asyncapi: "/path/to/asyncapi.json channelName"
---
```

## Playground configuration

Control the API playground behavior in `docs.json`:

```json
"api": {
  "playground": {
    "display": "interactive",
    "proxy": true
  },
  "examples": {
    "languages": ["bash", "javascript", "python"],
    "defaults": "all",
    "prefill": false,
    "autogenerate": true
  },
  "mdx": {
    "server": "https://api.example.com",
    "auth": {
      "method": "bearer"
    }
  }
}
```

- `playground.display`: `"interactive"`, `"simple"`, `"none"`, or `"auth"`.
- `playground.proxy`: Route requests through Mintlify's proxy. Default: `true`.
- `playground.credentials`: Include cookies and auth headers for cross-origin requests when proxy is `false`. Default: `false`.
- `params.expanded`: Expand all parameters by default. `"all"` or `"closed"` (default).
- `params.post`: OpenAPI schema field keys to surface as pills next to parameter names (array of strings).
- `url`: Set to `"full"` to always show the full base URL.
- `examples.languages`: `bash`, `go`, `java`, `javascript`, `node`, `php`, `powershell`, `python`, `ruby`, `swift`.
- `examples.defaults`: `"required"` or `"all"` (include optional params).
- `examples.prefill`: Pre-fill playground fields with spec example values. Default: `false`.
- `examples.autogenerate`: Generate code samples from API specs. Default: `true`.
- `mdx.auth.method`: `"bearer"`, `"basic"`, `"key"`, `"cobo"`.
