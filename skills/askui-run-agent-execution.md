---
name: Create an agent and run an execution
description: Register an AskUI agent and drive an agent execution to completion — the core loop for running a vision-agent automation via the AskUI Workspaces API.
api: openapi/askui-workspace-service-openapi-original.json
operations:
  - create_agent_api_v1_agents_post
  - create_agent_execution_api_v1_agent_executions_post
  - list_agent_executions_api_v1_agent_executions_get
  - update_agent_execution_api_v1_agent_executions__agent_execution_id__patch
---

# Create an agent and run an execution

Base host: `https://workspaces.askui.com` (paths are prefixed `/api/v1`).
Auth: `Authorization: Bearer <access-token>` (see `authentication/askui-authentication.yml`).

## Steps
1. **Create an agent** — `POST /api/v1/agents`
   (`create_agent_api_v1_agents_post`). Optionally start from a template listed by
   `GET /api/v1/agents/templates` (`list_agent_templates_api_v1_agents_templates_get`).
   Capture the `agent_id`.
2. **Start an execution** — `POST /api/v1/agent-executions`
   (`create_agent_execution_api_v1_agent_executions_post`) referencing the `agent_id`.
   Capture the `agent_execution_id`.
3. **Track progress** — `GET /api/v1/agent-executions`
   (`list_agent_executions_api_v1_agent_executions_get`), filtering by `status`. An
   execution moves through states such as pending-inputs, pending-review,
   pending-data-extraction, confirmed, canceled.
4. **Advance or resolve** — `PATCH /api/v1/agent-executions/{agent_execution_id}`
   (`update_agent_execution_api_v1_agent_executions__agent_execution_id__patch`) to
   confirm, supply inputs, or cancel a pending execution.

## Conventions
- Page results with `continuation_token` + `limit`; filter with `status` / `tags`.
- Errors are HTTP status codes with a FastAPI `detail` body (`422` -> HTTPValidationError);
  see `errors/askui-problem-types.yml`.
- Writes are not idempotent (no Idempotency-Key) — inspect state before retrying.
