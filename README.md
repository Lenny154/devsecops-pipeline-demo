# devsecops-pipeline-demo

> Phase 3 Project 1 — security scanning baked into every stage of CI/CD

![DevSecOps](https://img.shields.io/badge/DevSecOps-enabled-red)
![Trivy](https://img.shields.io/badge/Trivy-scanning-blue)
![Gitleaks](https://img.shields.io/badge/Gitleaks-protected-green)
![Node.js](https://img.shields.io/badge/Node.js-18-339933?logo=nodedotjs)

## Pipeline architecture

```
git push
    │
    ▼
Job 1 — Secret Scanning (Gitleaks)
    │ BLOCKS if API keys or passwords found in code
    ▼
Job 2 — SAST & Tests
    │ ESLint security plugin (dangerous code patterns)
    │ Jest tests + coverage must be ≥80%
    ▼
Job 3 — Build & Trivy Scan
    │ docker build
    │ Trivy scans image for CVEs
    │ BLOCKS if CRITICAL vulnerabilities found
    │ Pushes to Docker Hub only if clean
    ▼
Job 4 — Deploy to Render
    Only runs if ALL 3 security gates passed
```

## Security tools

| Tool | What it catches |
|------|----------------|
| Gitleaks | API keys, passwords, tokens committed to git |
| ESLint security | Dangerous JS patterns — injection, path traversal |
| Trivy | CVEs in Docker image, OS packages, npm deps |

## Secrets required

| Secret | Value |
|--------|-------|
| DOCKERHUB_USERNAME | Docker Hub username |
| DOCKERHUB_TOKEN | Docker Hub access token |
| RENDER_DEPLOY_HOOK | Render deploy webhook URL |

## Lessons learned
See [docs/lessons-learned.md](docs/lessons-learned.md)
