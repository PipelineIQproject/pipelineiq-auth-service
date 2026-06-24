# PipelineIQ Auth Service

Independent repository for the PipelineIQ authentication API.

## Build  

```bash
docker build -t <acr-login-server>/final_capstone-auth-service:local -f services/auth-service/Dockerfile .
``` 

## Local Run  

This service expects PipelineIQ environment variables from Kubernetes ConfigMap and Key Vault secrets.
 
```bash
cd services/auth-service
npm install
PORT=8081 DATABASE_URL=<postgres-url> npm start
```

## CI/CD Pipeline

Pipeline file: `.github/workflows/service-ci.yml`

Run it from GitHub Actions with `Run workflow` on the `dev` branch, or push to `dev`:

```bash
git checkout dev
git add .
git commit -m "change auth service"
git push origin dev
```

This repository calls the reusable workflow in `PipelineIQproject/pipeline_main`. The shared workflow runs lint/test/build when scripts exist, SonarQube Cloud, Snyk, Docker build, Trivy, smoke test on `/health`, ACR push, dev Helm value update, production approval, prod Helm value update, and Slack notification.

## Required Secrets

Add these GitHub Actions secrets in this repository:

| Secret | Purpose |
| --- | --- |
| `ACR_LOGIN_SERVER` | Azure Container Registry server. |
| `ACR_USERNAME` | Identity allowed to push to ACR. |
| `ACR_PASSWORD` | Password/secret for the ACR identity. |
| `SONAR_TOKEN` | SonarQube Cloud token. |
| `SNYK_TOKEN` | Snyk API token. |
| `MAIN_REPO_PAT` | PAT with write access to `PipelineIQproject/pipeline_main`. |
| `SLACK_WEBHOOK_URL` | Slack incoming webhook for success/failure notifications. |
| `SMOKE_TEST_ENV_FILE` | Dotenv content for local container smoke test, including `DATABASE_URL` and any auth secrets needed at startup. |

## Production Promotion

Create a GitHub Environment named `production` with required reviewers. After the dev image is pushed and `auth-service/values-dev.yaml` is updated in the main repo, GitHub pauses for approval before updating `auth-service/values-prod.yaml` on `master`.
