# AI provider configuration

Amy's backend talks to one configurable AI provider. The browser extension never sees the provider URL's credentials or the API key; only the backend reads them.

> **Verify everything against your provider's documentation:** endpoint URL, authentication header, model or deployment names, API version, quotas, rate limits and data-processing terms. Amy ships no provider URL, model name or header defaults.

## Provider types

| `AI_PROVIDER` | Behaviour |
| --- | --- |
| `mock` (default) | Simulated, deterministic responses. No external calls, no key. The UI shows "Demo mode". |
| `openai-compatible` | Sends an OpenAI-compatible chat-completions request to `AI_PROVIDER_BASE_URL`. Use it only if your provider documents support for this request format. |

Request format sent by `openai-compatible`:

```jsonc
// POST AI_PROVIDER_BASE_URL[?api-version=AI_PROVIDER_API_VERSION]
// <AI_PROVIDER_AUTH_HEADER>: [AI_PROVIDER_AUTH_SCHEME ]<AI_PROVIDER_API_KEY>
{ "model": "<AI_PROVIDER_MODEL, if set>", "messages": [{ "role": "system", "content": "..." }, { "role": "user", "content": "..." }] }
// expected response: { "choices": [{ "message": { "content": "<JSON>" } }], "usage": { ... } (optional) }
```

`anthropic-vertex` sends Claude's Anthropic Messages format to a Vertex AI-style endpoint. `AI_PROVIDER_API_VERSION`, `AI_PROVIDER_JSON_MODE` and `AI_PROVIDER_REASONING_EFFORT` don't apply; leave `AI_PROVIDER_TEMPERATURE` empty.

```jsonc
// POST AI_PROVIDER_BASE_URL/publishers/anthropic/models/<model>:rawPredict
// <AI_PROVIDER_AUTH_HEADER>: [AI_PROVIDER_AUTH_SCHEME ]<AI_PROVIDER_API_KEY>
{ "anthropic_version": "vertex-2023-10-16", "system": "...", "messages": [{ "role": "user", "content": "..." }], "max_tokens": 16000 }
// expected response: { "content": [{ "type": "text", "text": "<JSON>" }], "usage": { ... } (optional) }
```

**Amy's designer** (Mockup Designer) works with tools and streams its long answers from `:streamRawPredict` (the same path and headers, `"stream": true`, answered with server-sent events). When the provider or a gateway in front of it refuses that endpoint (401, 403, 404 or 405), the backend logs it and asks `:rawPredict` instead until it restarts: each answer must then arrive within `AI_PROVIDER_TIMEOUT_MS` and is capped at `AI_PROVIDER_MAX_OUTPUT_TOKENS`, so the designer writes long pages in parts. Allow `:streamRawPredict` on the gateway for the best results.

### Claude models: thinking and effort

Claude Opus 5.5, Sonnet 5.5 and Haiku 5.5 always think adaptively: they decide how much to think, and Opus 5.5 and Sonnet 5.5 refuse requests that switch thinking off. Configure them with `"thinking": "always"`, and older Claude models (4.5 and earlier) with `"thinking": "unsupported"`; Vertex AI takes the plain model names.

```
AI_MODELS=[{"id":"opus-5-5","label":"Claude Opus 5.5","model":"claude-opus-5-5","location":"cloud","thinking":"always","default":true},{"id":"sonnet-5-5","label":"Claude Sonnet 5.5","model":"claude-sonnet-5-5","location":"cloud","thinking":"always"},{"id":"haiku-5-5","label":"Claude Haiku 5.5","model":"claude-haiku-5-5","location":"cloud","thinking":"always"}]
AI_PROVIDER_MAX_OUTPUT_TOKENS=32000
AI_PROVIDER_TIMEOUT_MS=300000
```

**Effort.** Each quick action's effort in Personas (Low, Medium, High, Extra high, Max) is sent as `"output_config": {"effort": …}`. "Off" becomes Low, because Claude keeps thinking. Without an effort, the model's own default applies: Medium on Opus 5.5 and Haiku 5.5, High on Sonnet 5.5.

**Starting points:**
- Low for Ask Amy and work-item fields;
- High for Generate Project Documentation and pull-request reviews;
- Max for Debate Architecture.

Higher effort means more thinking: more output tokens, cost and time.

**Output tokens.** Thinking counts toward `max_tokens`. Keep `AI_PROVIDER_MAX_OUTPUT_TOKENS` at 32000 or more, so long answers aren't cut short, and allow a few minutes per request.

If your provider needs a different format, adapt `buildBody()` and `extractContent()` in `backend/src/providers/http-provider.ts`; nothing else depends on the wire format.

## Settings

| Variable | Secret | Required | Description |
| --- | --- | --- | --- |
| `AI_PROVIDER` | no | no | `mock` (default) or `openai-compatible` |
| `AI_PROVIDER_BASE_URL` | no | for a real provider | Full endpoint URL; `https://` required in production; must not contain credentials |
| `AI_PROVIDER_API_KEY` | **yes** | for a real provider (api-key mode) | Key Vault reference in production |
| `AI_PROVIDER_AUTH_HEADER` | no | for a real provider (api-key mode) | Header that carries the key, as documented by the provider |
| `AI_PROVIDER_MODEL` | no | only if the provider needs it | Sent as `"model"` |
| `AI_PROVIDER_API_VERSION` | no | only if the provider needs it | Sent as the `api-version` query parameter |
| `AI_PROVIDER_TIMEOUT_MS` | no | no | Per-request timeout, default `60000`, at most `1800000` (30 minutes, for slow local models); timeouts are not retried. Debates and pull-request reviews of large changes with thinking on may need 180000 or more |
| `AI_PROVIDER_AUTH_SCHEME` | no | no | Optional prefix for the header value (only if documented) |
| `AI_PROVIDER_MAX_RETRIES` | no | no | Retries for network errors and HTTP 408/429/5xx (default `2`, exponential backoff, honours `Retry-After` up to 10 s) |
| `AI_PROVIDER_CORRELATION_HEADER` | no | no | Forwards the request correlation ID, if the provider supports such a header |
| `AI_PROVIDER_JSON_MODE` | no | no | `true` adds `"response_format": {"type":"json_object"}`; enable only if supported |
| `AI_PROVIDER_TEMPERATURE` | no | no | Sent as `"temperature"` when set |
| `AI_PROVIDER_MAX_OUTPUT_TOKENS` | no | no | Sent as `"max_tokens"` when set |
| `AI_PROVIDER_REASONING_EFFORT` | no | no | `none`, `minimal`, `low`, `medium` or `high`, sent as `"reasoning_effort"` for reasoning models. `none` turns thinking off where the provider supports it |
| `AI_PROVIDER_AUTH_MODE` | no | no | `api-key` (default) or `entra` (see below) |
| `AI_PROVIDER_ENTRA_SCOPE` | no | for `entra` | Token scope for service-to-service authentication |

## Models users can choose (`AI_MODELS`)

Amy's settings (the logo in the hub header) let users pick a model from the list you approve in `AI_MODELS`, a JSON array on one line. All models are served by the endpoint in `AI_PROVIDER_BASE_URL`, with the same credentials; to offer models from several hosts, use a gateway (for example LiteLLM, or Ollama, which serves local and cloud models from one endpoint). Without `AI_MODELS`, users see one model: `AI_PROVIDER_MODEL`, marked Local when the endpoint is on `localhost` or a private network address, Cloud otherwise.

| Field | Required | Meaning |
| --- | --- | --- |
| `id` | yes | Identifier the extension sends (letters, digits, `.`, `_`, `:`, `-`) |
| `label` | yes | Name shown to users |
| `model` | yes | Model name sent to the provider; never shown to users |
| `location` | yes | `local` (your own infrastructure) or `cloud` |
| `thinking` | no | `switchable`, `always` or `unsupported` (default) |
| `thinkingSwitch` | for `switchable` | `reasoning-effort`: off sends `"reasoning_effort": "none"`, on sends nothing (the model's own default); for Ollama and OpenAI-style APIs. `chat-template`: sends `"chat_template_kwargs": {"enable_thinking": true/false}`; for vLLM and SGLang |
| `thinkingDefault` | no | Initial setting for `switchable` models (default `true`) |
| `default` | no | The organization's default model (at most one; otherwise the first) |

```
AI_MODELS=[{"id":"qwen3.5-4b","label":"Qwen3.5 4B","model":"qwen3.5-amy:4b","location":"local","thinking":"switchable","thinkingSwitch":"reasoning-effort","thinkingDefault":false}]
```

`AI_PROVIDER_REASONING_EFFORT` still applies to models without a switch. The thinking toggle is never mapped to effort levels. **Response length** is added to the prompt as a wording preference; `AI_PROVIDER_MAX_OUTPUT_TOKENS` stays the same, so structured output is never cut short.

## Web search (optional)

| Setting | Meaning |
| --- | --- |
| `WEB_SEARCH_PROVIDER` | `none` (default), `searxng` (self-hosted [SearXNG](https://docs.searxng.org/) with the JSON format enabled) or `brave` ([Brave Search API](https://brave.com/search/api/)) |
| `WEB_SEARCH_BASE_URL` | SearXNG base URL: `http://localhost:8888` for SearXNG next to the backend (below). Elsewhere https in production |
| `WEB_SEARCH_API_KEY` | [SECRET] Brave subscription token; a Key Vault reference in production |
| `WEB_SEARCH_MAX_RESULTS` | Results passed to the model, 1–10 (default 5) |
| `WEB_SEARCH_TIMEOUT_MS` | Default 10000 |

Web search is offered (capability `web-search`) only when a provider is configured and `ENABLED_OPERATIONS` allows it, and Amy then has it on by default; users can turn it off in Amy's settings. It applies to questions, Create Work Items, Design Requirements, Review a Pull Request, Debate Architecture, Ask with search and Diagnose a Ticket. The backend sends up to three search terms the AI model writes from the request (for a question, from the question with its personal data masked), naming only public products, standards or regulations; Ask with search and Diagnose a Ticket send the agent's search queries, at most 5 per answer. Since backend 0.15.2 a term or query is never sent when it names the organization, the project, a repository, Wiki, board, team or log service, or holds an address, a file path with folders, a GUID or masked personal data; the backend logs `Web search skipped` with `reason: internal-detail`. Nothing else is sent to the search service; review that service's data terms before enabling it. A question or term that looks like it contains a key, password, token or connection-string secret is never sent: the backend works without the web, with a warning, and logs `Web search skipped`. Logs record result counts and durations, never the query.

### SearXNG next to the backend

[SearXNG](https://docs.searxng.org/) is a free, self-hosted metasearch engine: it passes the question to public search engines (for example Bing, DuckDuckGo, Brave and Wikipedia) and returns their results. It runs as a sidecar container in the backend's App Service, from the `amy-searxng` image: only the backend reaches it, at `http://localhost:8888`, and it needs no extra Azure resources. Setup: [DEPLOY.md](DEPLOY.md#optional-web-search).

## Validation at startup

The backend refuses to start (exit code 1) when:

- a real provider is selected and `AI_PROVIDER_BASE_URL`, or in api-key mode `AI_PROVIDER_API_KEY` or `AI_PROVIDER_AUTH_HEADER`, is missing;
- a value is a placeholder (for example `<AI-PROVIDER-API-KEY>`, `changeme`, `your-...`);
- the URL is invalid, contains credentials, or is not `https://` in production;
- a setting still contains a literal `@Microsoft.KeyVault(...)` reference, which means App Service could not resolve it;
- production runs with `AUTH_MODE=none` or without `https://` `ALLOWED_ORIGINS`.

Error messages name the variable, never its value. After a successful start the backend logs a sanitized summary: environment, provider type, endpoint host name, model, timeout, whether a credential is configured, and the authentication modes.

## Production: Azure Key Vault reference

In production `AI_PROVIDER_API_KEY` is an App Service setting whose value is a Key Vault reference:

```text
AI_PROVIDER_API_KEY=@Microsoft.KeyVault(SecretUri=https://<key-vault-name>.vault.azure.net/secrets/ai-provider-api-key/)
```

App Service resolves the reference with its system-assigned managed identity and passes the secret to the process as an ordinary environment variable. The backend reads `process.env.AI_PROVIDER_API_KEY` once at startup, validates it, and injects it into the provider. There is no Key Vault SDK in the application. The reference is versionless, so new secret versions are picked up without changing settings. Setup steps are in [DEPLOY.md](DEPLOY.md).

## Entra ID instead of an API key

`AI_PROVIDER_AUTH_MODE=entra` sends `Authorization: Bearer <token>`, obtained for `AI_PROVIDER_ENTRA_SCOPE` from an `AccessTokenSource`. It is not wired by default. To use it, confirm that your provider accepts Entra tokens, add `@azure/identity`, and pass `new DefaultAzureCredential()` to `createProvider(config, tokenSource)`. Without that wiring the backend refuses to start in entra mode.

## Checking the connection

Health probes never call the provider. To test connectivity, for example after rotating the key, use the protected diagnostic endpoint:

1. Set `ADMIN_DIAGNOSTICS_TOKEN` (32+ characters, as a Key Vault reference in production).
2. Call:

   ```powershell
   Invoke-RestMethod -Method Post -Uri "https://<backend-app-service-name>.azurewebsites.net/api/admin/provider-check" -Headers @{ "x-admin-token" = $env:ADMIN_TOKEN }
   ```

It sends one minimal request, at most every 10 seconds, and returns only `ok`, a status category (`2xx`, `4xx`, `5xx`, `timeout`, `network`), the latency, the provider type and the model. It never returns upstream error text. Without the token setting the endpoint returns 404.
