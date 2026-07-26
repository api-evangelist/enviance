---
name: Authenticate and open an Enviance session
description: Obtain a SessionId from the Cority Enviance EMS REST API, verify the current session, and end it cleanly.
api: openapi/enviance-restapi-openapi-original.json
operations:
  - Enviance.RestServices.Ver2.Security.Impl.AuthenticationService.Authenticate
  - Enviance.RestServices.Ver2.Security.Impl.AuthenticationService.GetCurrentSessionInfo
  - Enviance.RestServices.Ver2.Security.Impl.AuthenticationService.EndCurrentSession
---

# Authenticate and open an Enviance session

Base URL: `https://api.enviance.com` — every path is namespaced under `/ver2/`.
Using the API requires a Cority Enviance Connectors license.

## Steps

1. **Authenticate** — `POST /ver2/AuthenticationService.svc/sessions`
   (`Authenticate`). Supply the account credentials in the request body. The
   response returns a `SessionId`. (Alternatives: `/sessions/cert` for
   certificate auth, `/sessions/long` for a long-lived session, or
   `/oauthtoken` for an OAuth access token.)
2. **Set the auth header** — send `Authorization: Enviance <SessionId>` on every
   subsequent request. HTTP Basic through the API gateway is also accepted.
3. **Verify** — `GET /ver2/AuthenticationService.svc/currentsession`
   (`GetCurrentSessionInfo`) confirms the session is live and returns its context.
4. **End the session** — `DELETE /ver2/AuthenticationService.svc/currentsession`
   (`EndCurrentSession`) when finished.

## Rules

- Include `x-client-request-id` on each call for correlation/tracing.
- Set `EnvApi-Systemid` / `EnvApi-Packageid` when targeting a specific system or package.
- There is no idempotency-key mechanism — do not assume safe automatic retries on writes.
- Errors return `{ error: string, details: object }`; branch on the `error` field.
