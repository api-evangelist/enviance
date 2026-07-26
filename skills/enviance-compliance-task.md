---
name: Create a compliance requirement and track its task
description: Register an environmental compliance requirement in Cority Enviance and create/read the compliance task that tracks it.
api: openapi/enviance-restapi-openapi-original.json
operations:
  - Enviance.RestServices.Ver2.Requirements.Impl.RequirementService.CreateRequirement
  - Enviance.RestServices.Ver2.Tasks.Impl.TaskService.CreateTask
  - Enviance.RestServices.Ver2.Tasks.Impl.TaskService.GetTask
---

# Create a compliance requirement and track its task

Base URL: `https://api.enviance.com`. Authenticate first (see
`enviance-authenticate-session.md`) and send `Authorization: Enviance <SessionId>`.

## Steps

1. **Create the requirement** — `POST /ver2/RequirementService.svc/requirements`
   (`CreateRequirement`). Provide the requirement definition (name/path, the
   location it applies to, and its compliance parameters) in the body.
2. **Create the tracking task** — `POST /ver2/TaskService.svc/tasks`
   (`CreateTask`). Reference the requirement and set the schedule/due dates so
   Enviance generates task occurrences.
3. **Read task status** — `GET /ver2/TaskService.svc/tasks/{taskId}` (`GetTask`)
   to inspect the task. Occurrences are available under
   `/tasks/{taskId}/occurrences/{dueDate}`, and `/tasks/{taskId}/ical` exports
   an iCalendar feed.

## Rules

- Set `EnvApi-Systemid` / `EnvApi-Packageid` to target the correct Enviance system.
- Include `x-client-request-id` for tracing.
- List/search endpoints paginate with `page` / `pageSize` / `limit`.
- No idempotency key exists — guard against duplicate creates yourself before retrying.
- Errors return `{ error: string, details: object }`.
