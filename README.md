# Keycloak Deployment Demo

Demo repository showing how to use [Keycloak Misconfiguration Analyzer](https://github.com/RoyMoskitov/keycloak-misconfig-analyzer) as a DAST tool in CI/CD pipeline.

## What happens

On every push, GitHub Actions:
1. Starts Keycloak from `docker-compose.yml`
2. Imports realm configuration from `realm-config.json`
3. Runs the security scanner against Keycloak
4. Outputs scan results as GitHub Actions summary
5. Fails the pipeline if critical issues are found

## Local usage

```bash
# Start Keycloak
docker-compose up -d

# Run scanner
docker run --rm --network host \
  ghcr.io/roymoskitov/keycloak-scanner:latest \
  --cli \
  --keycloak-audit.server-url=http://localhost:8080 \
  --keycloak-audit.realm=master \
  --keycloak-audit.client-id=admin-cli \
  --keycloak-audit.username=admin \
  --keycloak-audit.password=adminpass \
  --output=/dev/stdout \
  --format=json \
  --spring.main.web-application-type=none
```
