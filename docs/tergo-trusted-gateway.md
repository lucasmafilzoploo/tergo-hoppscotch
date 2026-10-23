# Trusted gateway sign-in

This fork adds an optional `GET /v1/auth/trusted-gateway` endpoint and a
self-hosted-web automatic sign-in step. It is **disabled by default**.

Set both variables only when all backend traffic is behind a reverse-auth
gateway that authenticates the request and replaces the following headers:

- `HOPPSCOTCH_TRUSTED_GATEWAY_AUTH=true` for the backend.
- `VITE_TRUSTED_GATEWAY_AUTH=true` for the web client.

The gateway must provide `X-Auth-Request-Email` or `X-Forwarded-Email`; it
may provide `X-Auth-Request-User` as a display name. The backend creates or
finds the local account and emits only Hoppscotch's own HTTP-only session
cookies. It does not store provider access or refresh tokens.

Do not expose the backend directly, and do not enable this mode with a proxy
that lets clients supply those headers unchanged. A missing or invalid email
fails closed with `401`.
