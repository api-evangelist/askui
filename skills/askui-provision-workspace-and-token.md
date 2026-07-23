---
name: Provision a workspace and mint an access token
description: Create an AskUI workspace, mint a workspace-scoped access token, and add a member — the setup an agent needs before running automations against the AskUI Workspaces API.
api: openapi/askui-workspace-service-openapi-original.json
operations:
  - create_workspace_api_v1_workspaces_post
  - create_access_token_api_v1_workspaces__workspace_id__access_tokens_post
  - create_workspace_membership_api_v1_workspace_memberships_post
---

# Provision a workspace and mint an access token

Base host: `https://workspaces.askui.com` (paths are prefixed `/api/v1`).

## Auth
Send `Authorization: Bearer <access-token>`. Credentials are a workspace id + access
token created in AskUI Hub. See `authentication/askui-authentication.yml`.

## Steps
1. **Create the workspace** — `POST /api/v1/workspaces`
   (`create_workspace_api_v1_workspaces_post`). Capture the returned `workspace_id`.
2. **Mint a workspace access token** — `POST /api/v1/workspaces/{workspace_id}/access-tokens`
   (`create_access_token_api_v1_workspaces__workspace_id__access_tokens_post`). Store
   the token secret securely; AskUI deletes tokens immediately with no grace period.
3. **Add a member** (optional) — `POST /api/v1/workspace-memberships`
   (`create_workspace_membership_api_v1_workspace_memberships_post`).

## Conventions
- Errors are standard HTTP status codes with a FastAPI `detail` body; `422` returns
  `HTTPValidationError`. See `errors/askui-problem-types.yml`.
- No idempotency-key header is supported — do not retry blindly on write failures.
- List endpoints page with `continuation_token` + `limit`.
