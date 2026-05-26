# Lessons learned

## What I built
A DevSecOps pipeline with 4 security gates that must all pass before deployment:
- Gitleaks: scans every commit for accidentally committed secrets
- ESLint security plugin: catches dangerous JavaScript code patterns
- Jest: functional tests with 80% coverage enforcement
- Trivy: scans Docker image for known CVEs — blocks on CRITICAL

## Key concepts

### Shift left security
Traditional security: review before release (catches problems too late)
DevSecOps: automated security checks on every single commit
Result: vulnerabilities caught in minutes not months

### Gitleaks
Scans git history for secrets using regex patterns.
Catches API keys, passwords, tokens, private keys.
Blocks the entire pipeline if any secret is found.

### Trivy
Scans Docker images against CVE databases (NVD, GitHub Advisory).
exit-code 1 means pipeline FAILS if CRITICAL CVEs found.
ignore-unfixed skips CVEs with no available fix yet.
Also outputs SARIF format for GitHub Security tab.

### ESLint security plugin
Catches dangerous patterns like:
- Object injection (prototype pollution attacks)
- Non-literal filesystem paths (path traversal)
- Timing attacks in comparisons

### Pipeline gate principle
Each job chains with needs: — if any gate fails, everything stops.
Nothing reaches production unless ALL security checks pass.
This is non-negotiable in enterprise environments.

## Run locally
npm audit                                    # npm dependency check
trivy image bucle154/devsecops-demo:latest   # image CVE scan
trivy fs .                                   # filesystem scan
gitleaks detect --source .                   # secret scan
