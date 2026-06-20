# PipelineIQ Auth Service

Independent repository staging folder for the PipelineIQ auth service.

## Build

```bash
docker build -t nimeshsv814/pipelineiq-auth-service:v1.0.0 -f services/auth-service/Dockerfile .
```

## Run

This service expects PipelineIQ environment variables from Kubernetes ConfigMap and Key Vault secrets.
