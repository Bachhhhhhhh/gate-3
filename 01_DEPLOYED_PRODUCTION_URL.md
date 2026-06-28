# Gate 3 Deliverable: Deployed Production URL

## Product

Inspectra is a deployed web app for AI-assisted surface defect inspection. Users
can log in, upload an inspection image, run VLM-based defect detection, and view
user-owned reports with defect evidence and reviewer status.

## Production URLs

| Component | URL | Purpose |
|---|---|---|
| Web UI | https://c2-app-089-production.up.railway.app | Main user-facing product |
| Backend API | https://inspectra-api-production.up.railway.app | Auth, reports, uploads, VLM report API |
| API health check | https://inspectra-api-production.up.railway.app/api/health | Production availability check |

## Demo Accounts

| Role | Email | Password |
|---|---|---|
| Admin | `admin@inspectra.ai` | `admin123` |
| Inspector | `inspector@inspectra.ai` | `inspect123` |
| Operator | `operator@inspectra.ai` | `operator123` |

## Production Smoke Evidence

Backend health check:

```json
{"status":"ok","service":"inspectra-api","version":"2.0.0"}
```

Production web upload smoke test:

```text
Report ID: VLM-260628-3594
Status: fail
Defect count: 2
Artifacts: result_json, source_image, segmentation_overlay, overlay
```

This confirms that the deployed product can:

- Authenticate a user.
- Accept a local image upload.
- Call the VLM inspection flow.
- Save report artifacts.
- Show the result in the user's report history.

## Important URL Note

The frontend URL is the web app users should open:

```text
https://c2-app-089-production.up.railway.app
```

The backend URL is used by CLI/API integrations:

```text
https://inspectra-api-production.up.railway.app
```

Opening the backend root path may show `{"detail":"Not Found"}` because the API
does not define a `/` route. The correct health route is `/api/health`.
