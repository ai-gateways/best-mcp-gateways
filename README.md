# Best MCP Gateways

An MCP gateway is a single entry point between MCP clients, such as coding agents and IDEs, and the Model Context Protocol servers that give them tools and data. It authenticates each caller, limits which tools that caller can reach, handles upstream credentials, and records tool calls. Products using the label range from local developer tools to organization-wide control planes.

This list selects 14 MCP gateways that meet the criteria below. Each entry gives the license and deployment model, what the gateway does well, a real limitation, and who it suits. The full catalog is [Awesome AI Gateways](https://github.com/ai-gateways/awesome-ai-gateways#mcp-gateways), and the [MCP gateway guide](https://github.com/ai-gateways/mcp-gateway#what-is-an-mcp-gateway) explains how the technology works.

## Contents

- [How these MCP gateways were selected](#how-these-mcp-gateways-were-selected)
- [MCP gateway comparison](#mcp-gateway-comparison)
- [Best open-source MCP gateways](#best-open-source-mcp-gateways)
  - [Bifrost](#bifrost)
  - [LiteLLM](#litellm)
  - [agentgateway](#agentgateway)
  - [Kong AI Gateway](#kong-ai-gateway)
  - [ContextForge](#contextforge)
  - [Archestra](#archestra)
  - [MCPHub](#mcphub)
  - [ToolHive](#toolhive)
  - [Docker MCP Gateway](#docker-mcp-gateway)
  - [MCPJungle](#mcpjungle)
  - [Obot](#obot)
  - [MCP Gateway and Registry](#mcp-gateway-and-registry)
- [Best managed MCP gateways](#best-managed-mcp-gateways)
  - [Amazon Bedrock AgentCore Gateway](#amazon-bedrock-agentcore-gateway)
  - [Cloudflare MCP server portals](#cloudflare-mcp-server-portals)
- [How to choose an MCP gateway](#how-to-choose-an-mcp-gateway)
- [What this list leaves out](#what-this-list-leaves-out)
- [Related guides](#related-guides)

## How these MCP gateways were selected

A platform is on this list only when its own repository or documentation shows that it meets every test below. Features in a paid edition count toward the tests, and each entry says which ones are paid.

- **One endpoint across servers.** Clients reach tools from multiple MCP servers through a single gateway endpoint.
- **Caller authentication.** The gateway authenticates callers with keys, tokens, OAuth, or single sign-on.
- **Scoped access.** Access to servers or tools can be limited per client, user, team, key, or group.
- **Upstream credentials.** The gateway stores or brokers credentials for upstream servers, so clients do not hold them.
- **Tool-call visibility.** Logs, metrics, or traces record tool calls.
- **Adoption and upkeep.** Open-source projects need at least 1,000 GitHub stars and a commit in the last six months. Closed products need self-serve access, public documentation, and a large existing user base through the vendor's cloud or network.

## MCP gateway comparison

The table compares the 14 gateways on license, deployment, caller authentication, access scope, upstream credentials, and other traffic they govern. A dash means the sources reviewed for this list do not document that traffic type, not that the feature is absent. Each entry below links the source for its row.

| Gateway | License | Deployment | Caller authentication | Access scope | Upstream credentials | Also governs |
|---|---|---|---|---|---|---|
| [Bifrost](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/overview.mdx#L134) | Apache 2.0 | Self-hosted | Virtual keys; SSO is Enterprise | Per client, request, or virtual key | Headers, OAuth, per-user OAuth or headers; token exchange is Enterprise | LLM, A2A |
| [LiteLLM](https://docs.litellm.ai/docs/mcp#:~:text=control%20MCP%20access%20by%20Key) | MIT, except `enterprise/` | Self-hosted | Virtual keys | Per key, team, or organization | OAuth, API key or bearer headers | LLM, A2A |
| [agentgateway](https://github.com/agentgateway/agentgateway#key-features) | Apache 2.0 | Self-hosted binary or Kubernetes | JWT, API keys, OAuth | CEL rules per tool, prompt, or resource | Backend authentication, token exchange | LLM, A2A |
| [Kong AI Gateway](https://developer.konghq.com/plugins/ai-mcp-proxy/#why-use-the-ai-mcp-proxy-plugin) | Apache 2.0 core; MCP plugins Enterprise | Self-hosted or Konnect | OAuth 2.0 plugin | Per-tool ACLs by consumer or group | Token exchange | LLM, A2A |
| [ContextForge](https://github.com/IBM/mcp-context-forge#overview--goals) | Apache 2.0 | PyPI, Docker, or Kubernetes | JWT, Basic, SSO | Virtual servers | User-scoped OAuth tokens | A2A, REST, gRPC |
| [Archestra](https://github.com/archestra-ai/archestra#what-it-does) | AGPL-3.0, plus Enterprise-licensed parts | Docker or Helm | SSO, virtual keys | RBAC and server permissions | OAuth and on-behalf-of | LLM, A2A |
| [MCPHub](https://github.com/samanhappy/mcphub#control-access-and-credentials) | Apache 2.0 | Docker | OAuth 2.0, bearer keys | Server and group visibility | Per-user credentials | - |
| [ToolHive](https://github.com/stacklok/toolhive#-gateway) | Apache 2.0 | Desktop or Kubernetes operator | OIDC or OAuth | Cedar policies, tool filtering | Backend authentication | - |
| [Docker MCP Gateway](https://github.com/docker/mcp-gateway#features) | MIT | Docker CLI plugin | Bearer token on HTTP transports | Tools enabled per profile | Docker Desktop secrets, OAuth | - |
| [MCPJungle](https://github.com/mcpjungle/MCPJungle#why-mcpjungle) | MPL-2.0 | Self-hosted | Client tokens in enterprise mode | Tool groups, per-client allow-lists | Bearer token, headers, OAuth | - |
| [Obot](https://github.com/obot-platform/obot#mcp-gateway) | MIT | Docker or Kubernetes | Identity provider sign-in | Per user or group | OAuth, user and shared credentials | LLM |
| [MCP Gateway and Registry](https://github.com/agentic-community/mcp-gateway-registry#whats-in-the-box) | Apache 2.0 | Docker Compose, EKS, or ECS | OAuth against an identity provider | Scopes, per-tool on virtual servers | Per-user egress auth | A2A discovery |
| [Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html#gateway-benefits-using) | Proprietary | AWS service | Inbound authentication | Cedar policies | OAuth flows, credential storage | LLM, A2A |
| [Cloudflare MCP server portals](https://developers.cloudflare.com/cloudflare-one/access-controls/ai-controls/mcp-portals/#key-features) | Proprietary | Managed | Cloudflare Access | Curated tools per portal, Access policies | Per-user OAuth to each server | - |

Details reflect each project's repository or documentation in November 2026; confirm current capabilities before choosing.

## Best open-source MCP gateways

Twelve open-source MCP gateways meet the criteria. They differ in scope: some govern only tool traffic, while others also handle model and agent calls. They also differ in how much identity and audit functionality sits in a paid edition or a stricter operating mode.

### Bifrost

[Bifrost](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/overview.mdx#L134), built by Maxim AI, exposes connected MCP servers through one `/mcp` endpoint and governs tool calls under the same virtual keys as [model calls](https://github.com/maximhq/bifrost#key-features) and [agent traffic](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/overview.mdx#L10).

- **License and deployment:** [Apache 2.0](https://github.com/maximhq/bifrost#license). Self-hosted through [npx, Docker, or the Go SDK](https://github.com/maximhq/bifrost#getting-started-options), or on [Kubernetes and other container platforms](https://github.com/maximhq/bifrost/blob/dev/docs/deployment-guides/overview.mdx#L11).
- **Does well:** [Upstream auth types](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/auth/overview.mdx#L19) include headers, OAuth 2.0, per-user OAuth, and per-user headers, and [tool filtering](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/filtering.mdx#L8) applies per client, per request, and [per virtual key](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/overview.mdx#L370). In the project's [Code Mode benchmark](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/code-mode.mdx#L36) with 508 tools across 16 servers, input tokens fell 92.8% and estimated cost fell from $377 to $29, with the same 65 of 65 pass rate.
- **Limitation:** [Per-user auth applies only to HTTP connections](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/auth/overview.mdx#L15). [Token exchange](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/auth/token-exchange.mdx#L19) requires an enterprise SCIM identity provider, and [role-based access control](https://github.com/maximhq/bifrost/blob/dev/docs/enterprise/rbac.mdx#L7) and [audit logs](https://github.com/maximhq/bifrost/blob/dev/docs/enterprise/audit-logs.mdx#L7) are Enterprise features.
- **Suits:** Teams with large tool sets that want MCP governed under the same keys and budgets as their model traffic.

### LiteLLM

[LiteLLM](https://docs.litellm.ai/docs/mcp#:~:text=control%20MCP%20access%20by%20Key) offers an MCP gateway inside its proxy server, with a fixed endpoint for all MCP tools.

- **License and deployment:** [MIT, except the `enterprise/` directory](https://github.com/BerriAI/litellm/blob/main/LICENSE#L3). Self-hosted with [Docker or Terraform](https://github.com/BerriAI/litellm#get-started).
- **Does well:** [Permissions by key, team, or organization](https://docs.litellm.ai/docs/mcp_control#:~:text=Control%20which%20MCP%20servers%20and%20tools%20can%20be%20accessed), Streamable HTTP, SSE, and stdio transports, and [MCP cost tracking](https://docs.litellm.ai/docs/mcp_cost), all alongside its LLM gateway.
- **Limitation:** [Stdio servers do not start](https://docs.litellm.ai/docs/mcp#:~:text=LITELLM_ENABLE_MCP_STDIO) until `LITELLM_ENABLE_MCP_STDIO=true` is set, and [single sign-on and audit logs](https://docs.litellm.ai/docs/enterprise#:~:text=single%20sign-on%2C%20audit%20logs) require the Enterprise license.
- **Suits:** Teams already running the LiteLLM proxy that want MCP access under the same keys and teams.

### agentgateway

[agentgateway](https://github.com/agentgateway/agentgateway#key-features) is a Rust proxy and [Linux Foundation](https://github.com/agentgateway/agentgateway#:~:text=Linux%20Foundation) project built on MCP and A2A.

- **License and deployment:** Apache 2.0. Self-hosted as a [standalone binary or on Kubernetes](https://github.com/agentgateway/agentgateway#documentation).
- **Does well:** [Multiplexing](https://agentgateway.dev/docs/standalone/latest/integrations/mcp/servers/virtual/) federates tools from several servers into one, across stdio, HTTP, SSE, and Streamable HTTP, with JWT, API key, and OAuth authentication and OpenTelemetry output.
- **Limitation:** [Tool access rules are written as CEL expressions](https://agentgateway.dev/docs/standalone/latest/documentation/mcp/mcp-authz/), so someone on the team has to author and review policy as code.
- **Suits:** Kubernetes platform teams comfortable with policy as code.

### Kong AI Gateway

[Kong AI Gateway](https://developer.konghq.com/plugins/ai-mcp-proxy/#why-use-the-ai-mcp-proxy-plugin) governs MCP through the AI MCP Proxy plugin, which proxies MCP servers, converts REST APIs into MCP tools, or exposes grouped tools as one MCP server.

- **License and deployment:** [Apache 2.0](https://github.com/Kong/kong#license) for Kong Gateway; the MCP plugins are part of AI Gateway Enterprise. [Self-hosted](https://github.com/Kong/kong#getting-started) or on Kong Konnect.
- **Does well:** [Per-tool ACLs](https://developer.konghq.com/plugins/ai-mcp-proxy/#acl-tool-control) by consumer or consumer group, plus [OAuth 2.0 token validation](https://developer.konghq.com/plugins/ai-mcp-oauth2/#purpose-and-core-functionality) and [token exchange](https://developer.konghq.com/plugins/ai-mcp-oauth2/#token-exchange).
- **Limitation:** Both plugins are [AI Gateway Enterprise features](https://developer.konghq.com/plugins/ai-mcp-proxy/#:~:text=only%20available%20as%20part%20of%20our%20AI%20Gateway%20Enterprise), and the AI MCP Proxy [cannot be configured with other AI plugins](https://developer.konghq.com/plugins/ai-mcp-proxy/#:~:text=Do%20not%20configure%20the%20AI%20MCP%20Proxy%20plugin%20together%20with%20other%20AI%20plugins) on the same Service or Route.
- **Suits:** Enterprise Kong customers that want MCP traffic under existing gateway policy.

### ContextForge

[ContextForge](https://github.com/IBM/mcp-context-forge#overview--goals), from IBM, is a registry and proxy that federates MCP servers, A2A agents, and REST or gRPC APIs into one endpoint.

- **License and deployment:** Apache 2.0. Installs from PyPI or Docker and scales to Kubernetes with Redis-backed federation.
- **Does well:** Virtual servers that bundle selected tools, broad transport coverage, [user-scoped OAuth tokens](https://github.com/IBM/mcp-context-forge#overview--goals), OpenTelemetry tracing, and 40+ plugins.
- **Limitation:** The README notes that [arm64 is not supported in production](https://github.com/IBM/mcp-context-forge#:~:text=arm64%20is%20not%20supported%20on%20production) and lists [settings with insecure defaults](https://github.com/IBM/mcp-context-forge#:~:text=have%20insecure%20defaults) that must be changed before production use.
- **Suits:** Teams that need to federate MCP alongside existing REST and gRPC services.

### Archestra

[Archestra](https://github.com/archestra-ai/archestra#what-it-does) is a platform with MCP, LLM, and A2A gateways, a private MCP registry, and an agent runtime.

- **License and deployment:** [AGPL-3.0, with parts under an Enterprise license](https://github.com/archestra-ai/archestra/blob/main/LICENSE.md#rules-first-match-wins). Runs with Docker or Helm on Kubernetes.
- **Does well:** MCP [OAuth and on-behalf-of authentication](https://github.com/archestra-ai/archestra#what-it-does) so tools run as the user, SSO, RBAC, deterministic guardrails for tool calls, and OpenTelemetry traces.
- **Limitation:** [Resource sharing is an Enterprise feature](https://archestra.ai/docs/administration/access-control#share-a-resource), and the codebase mixes AGPL and Enterprise-licensed files.
- **Suits:** Security-focused teams that want an MCP gateway inside a wider agent platform.

### MCPHub

[MCPHub](https://github.com/samanhappy/mcphub#connect-once-expose-everywhere) is a self-hosted gateway that connects local and remote MCP servers once and serves them through stable endpoints.

- **License and deployment:** Apache 2.0. Self-hosted with [Docker](https://github.com/samanhappy/mcphub#docker-deployment).
- **Does well:** Routes for groups or individual servers, semantic tool routing, [per-user credentials on shared servers, and OAuth 2.0 in client and server modes](https://github.com/samanhappy/mcphub#control-access-and-credentials), and [tool-call logs](https://github.com/samanhappy/mcphub#operate-with-confidence).
- **Limitation:** [Multiple replicas need explicit session affinity](https://github.com/samanhappy/mcphub#operate-with-confidence) and a shared PostgreSQL configuration.
- **Suits:** Small and midsize teams that want a straightforward self-hosted gateway with a dashboard.

### ToolHive

[ToolHive](https://github.com/stacklok/toolhive#-gateway), from Stacklok, runs each MCP server in an isolated container and adds a gateway, registry, and runtime.

- **License and deployment:** [Apache 2.0](https://github.com/stacklok/toolhive#license). Runs [on the desktop or through a Kubernetes operator](https://github.com/stacklok/toolhive#flexible-deployment).
- **Does well:** Virtual MCP endpoints that combine tools, [separate client and backend authentication](https://docs.stacklok.com/toolhive/guides-vmcp/authentication), identity provider integration, tool filtering, and audit logs with OpenTelemetry traces.
- **Limitation:** Identity and access policy are [enforced only when configured with an authentication source](https://github.com/stacklok/toolhive#:~:text=when%20configured%20with%20an%20authentication%20source), so a default install does not provide them.
- **Suits:** Platform teams that want container isolation for every server and declarative Kubernetes policy.

### Docker MCP Gateway

[Docker MCP Gateway](https://github.com/docker/mcp-gateway#features) is a Docker CLI plugin that runs catalog MCP servers in isolated containers and serves them through one gateway.

- **License and deployment:** MIT. Runs as a [Docker CLI plugin](https://github.com/docker/mcp-gateway#install-as-docker-cli-plugin).
- **Does well:** Container isolation, [secrets through Docker Desktop and built-in OAuth flows](https://github.com/docker/mcp-gateway#features), [bearer tokens on HTTP transports](https://github.com/docker/mcp-gateway/blob/main/docs/security.md#http-transports), and [tools enabled per profile](https://github.com/docker/mcp-gateway#profile-management).
- **Limitation:** The [listed prerequisite is Docker Desktop 4.59 or later with the MCP Toolkit enabled](https://github.com/docker/mcp-gateway#prerequisites), and its [security model trusts the local OS user and Docker Desktop](https://github.com/docker/mcp-gateway/blob/main/docs/security.md#trust-assumptions), so access is scoped per profile rather than per user or team.
- **Suits:** Developers and small teams that already use Docker and want isolated local MCP servers.

### MCPJungle

[MCPJungle](https://github.com/mcpjungle/MCPJungle#why-mcpjungle) is a self-hosted gateway that registers servers once and exposes them through one endpoint.

- **License and deployment:** MPL-2.0. Self-hosted, locally or as shared infrastructure.
- **Does well:** [Tool groups](https://docs.mcpjungle.com/guides/tool-groups), [per-client access control](https://docs.mcpjungle.com/governance/access-control), and [OpenTelemetry metrics](https://docs.mcpjungle.com/deployment/observability) in a small deployment.
- **Limitation:** [Authentication and client allow-lists require enterprise mode](https://docs.mcpjungle.com/governance/overview#development-mode-versus-enterprise-mode), and [upstream credentials are set per server at registration](https://docs.mcpjungle.com/governance/upstream-authentication).
- **Suits:** Developers who want one endpoint today and a path to shared use later.

### Obot

[Obot](https://github.com/obot-platform/obot#mcp-gateway) is an open-source platform with MCP and LLM gateways, MCP and skills registries, and isolated hosting for MCP servers.

- **License and deployment:** [MIT](https://github.com/obot-platform/obot#license). Self-hosted with Docker or Kubernetes.
- **Does well:** Virtual MCP servers, [access by user or identity-provider group](https://github.com/obot-platform/obot#identity-and-access-control), managed MCP OAuth and shared credentials, and [correlated audit logs](https://github.com/obot-platform/obot#audit-logs-and-visibility).
- **Limitation:** Free editions [cap users and devices at 100, and enterprise auth providers need a paid license key](https://docs.obot.ai/enterprise/overview#editions). Teams that only need a gateway still operate a wider platform.
- **Suits:** Organizations that want MCP, skills, and hosted server governance in one self-hosted platform.

### MCP Gateway and Registry

[MCP Gateway and Registry](https://github.com/agentic-community/mcp-gateway-registry#whats-in-the-box) is a governed entry point and registry for MCP servers, agents, and skills.

- **License and deployment:** [Apache 2.0](https://github.com/agentic-community/mcp-gateway-registry#license). Deploys with Docker Compose, on Amazon EKS with Helm, or on Amazon ECS with Terraform.
- **Does well:** OAuth against an existing identity provider with fine-grained scopes, virtual MCP servers with per-tool access control, [per-user egress authentication](https://github.com/agentic-community/mcp-gateway-registry#whats-in-the-box), semantic tool discovery, and audit logs.
- **Limitation:** [A2A traffic runs peer-to-peer by default](https://github.com/agentic-community/mcp-gateway-registry#how-it-works) rather than through the gateway, and the [installation guide](https://github.com/agentic-community/mcp-gateway-registry#quick-start) includes MongoDB and Keycloak setup.
- **Suits:** AWS-oriented teams that want a registry and discovery layer alongside the gateway.

## Best managed MCP gateways

Two managed services meet the criteria. They remove the work of running a gateway, but they tie MCP access to one vendor's cloud or network.

### Amazon Bedrock AgentCore Gateway

[Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html#gateway-benefits-using) is a managed AWS entry point that turns APIs and Lambda functions into MCP tools, fronts existing MCP servers, and routes agent and model traffic.

- **License and deployment:** Proprietary. A managed AWS service.
- **Does well:** Inbound and outbound authentication in one service, including OAuth flows, token refresh, and credential storage, [Cedar policies evaluated before tool access](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy.html#policy), semantic tool search, and [CloudWatch-based observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html#observability).
- **Limitation:** AWS only. For MCP server targets, [DYNAMIC listing mode does not work with semantic search or outbound three-legged OAuth](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-MCPservers.html#gateway-target-MCPservers-considerations).
- **Suits:** AWS teams that want MCP access without running a gateway.

### Cloudflare MCP server portals

[Cloudflare MCP server portals](https://developers.cloudflare.com/cloudflare-one/access-controls/ai-controls/mcp-portals/#key-features) centralize multiple MCP servers onto one HTTP endpoint behind Cloudflare Access.

- **License and deployment:** Proprietary. Managed by Cloudflare.
- **Does well:** Sign-in through Cloudflare Access, [per-user OAuth to each upstream server, curated tools per portal](https://developers.cloudflare.com/cloudflare-one/access-controls/ai-controls/mcp-portals/#key-features), and [portal logs](https://developers.cloudflare.com/cloudflare-one/access-controls/ai-controls/mcp-portals/#view-portal-logs) per portal or per server.
- **Limitation:** [Known limitations](https://developers.cloudflare.com/cloudflare-one/access-controls/ai-controls/mcp-portals/#known-limitations) include support for remote HTTP servers only, a 60-second timeout on upstream tool calls, and a maximum of 120 servers per portal.
- **Suits:** Organizations already using Cloudflare Access that want MCP behind existing sign-in.

## How to choose an MCP gateway

Choose an MCP gateway by how many people share it, whose identity upstream tools should see, and whether model calls need the same controls. Start from the situation closest to yours, then test with your own servers and clients.

| Situation | Candidates |
|---|---|
| Upstream tools must act as the requesting user | Bifrost, ContextForge, Archestra, MCPHub, Obot, MCP Gateway and Registry, Amazon Bedrock AgentCore Gateway, Cloudflare MCP server portals |
| One set of keys for MCP and model traffic | Bifrost, LiteLLM, agentgateway, Archestra, Obot |
| Container isolation for every server | ToolHive, Docker MCP Gateway |
| Large tool sets where token cost matters | Bifrost, ToolHive, Obot |
| Local development or a small team | Docker MCP Gateway, MCPJungle, MCPHub |
| Extending a gateway you already run | Kong AI Gateway |
| No gateway infrastructure to operate | Amazon Bedrock AgentCore Gateway, Cloudflare MCP server portals |

Run the candidates against real clients and at least one OAuth-protected server, and confirm how tool lists change for callers with different permissions. The [MCP gateway evaluation checklist](https://github.com/ai-gateways/mcp-gateway#how-to-evaluate-an-mcp-gateway) lists what else to check.

## What this list leaves out

This list covers gateways that meet every test above, so several related tools are out of scope. The catalog in this organization lists them.

- **Single-server proxies.** Products that put a gateway in front of each MCP server, without one endpoint across servers, do not meet the first test.
- **Smaller projects.** Open-source MCP gateways below the adoption bar are in [MCP Gateways](https://github.com/ai-gateways/awesome-ai-gateways#mcp-gateways).
- **Sales-led products.** Closed products without self-serve access are in [MCP Gateways](https://github.com/ai-gateways/awesome-ai-gateways#mcp-gateways) and [API Gateways with AI Features](https://github.com/ai-gateways/awesome-ai-gateways#api-gateways-with-ai-features).
- **Agent-only gateways.** Products focused on agent-to-agent traffic are in [Agent Gateways](https://github.com/ai-gateways/awesome-ai-gateways#agent-gateways).

## Related guides

- [MCP gateway security](https://github.com/ai-gateways/mcp-gateway#mcp-gateway-security): token handling and tool poisoning on the tool hop
- [Reducing token cost from large tool sets](https://github.com/ai-gateways/mcp-gateway#reducing-token-cost-from-large-tool-sets): filtering and code-based tool orchestration
- [Agent gateway vs MCP gateway vs LLM gateway](https://github.com/ai-gateways/agent-gateway#agent-gateway-vs-mcp-gateway-vs-llm-gateway): the three hops a single agent task crosses
- [Best AI Gateways](https://github.com/ai-gateways/best-ai-gateways#best-open-source-ai-gateways): the same selective treatment for AI gateways
- [Guardrails for tools and agents](https://github.com/ai-gateways/ai-guardrails#guardrails-for-tools-and-agents): checking tool arguments and tool results
- [Shadow AI](https://github.com/ai-gateways/ai-governance#shadow-ai): unreviewed MCP servers and AI tools outside the gateway
- [Awesome AI Gateways](https://github.com/ai-gateways/awesome-ai-gateways#mcp-gateways): the full catalog of MCP gateways
