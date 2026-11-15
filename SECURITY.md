# Security Policy

## Reporting a Vulnerability

If you find a security issue in shedeploys.dev, please report it privately rather than opening a public issue:

- Use [GitHub's private vulnerability reporting](https://github.com/agapasieka/shedeploys.dev/security/advisories/new).

Please include steps to reproduce and the potential impact. This is a small, single-maintainer project, so I'll aim to acknowledge reports within a few days.

## Scope

shedeploys.dev is a static site (HTML/CSS/JS) served by Firebase Hosting and deployed by GitHub Actions. It has no backend, no database and no user accounts. In scope:

- XSS or other client-side injection
- Weaknesses in the CI/CD workflows (e.g. ways to run code or read secrets through a pull request)
- Secrets or credentials committed to the repo

Out of scope: third-party sites the page links to (Medium, GitHub, LinkedIn, the linked projects), and denial-of-service against Firebase Hosting itself.
