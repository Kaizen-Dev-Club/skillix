# Security Policy

The Skillix team and contributors take the security of our platform and user data seriously. This document outlines our vulnerability disclosure process and reporting guidelines.

## Supported Versions

As an early-stage open-source platform, security patches are applied directly to the active `main` branch.

| Version | Supported          |
| :------ | :----------------- |
| `0.1.x` (main) | :white_check_mark: |
| `< 0.1.0`      | :x:                |

## Reporting a Vulnerability

If you discover a security vulnerability within Skillix, **please do not disclose it publicly** through public GitHub issues, discussions, or social media.

Instead, report it responsibly via:
- **GitHub Security Advisory**: Open a private advisory draft directly under the repository's "Security" tab.
- **Maintainer Email**: Contact the repository maintainers directly at the contact address listed in GitHub profiles or club channels.

### Information to Include in Your Report
To help us triage and resolve the issue quickly, please provide:
1. A detailed description of the vulnerability and its potential impact.
2. Step-by-step instructions or proof-of-concept (PoC) code to reproduce the issue.
3. Affected components (`frontend`, `backend`, middleware, database, or API routes).
4. Any proposed remediations or patches if available.

## Response SLA

- **Initial Acknowledgment**: Within 48 hours of receipt.
- **Triage & Assessment**: Within 5 business days.
- **Patch Development & Release**: High-severity issues are addressed promptly, followed by a public security release.

## Contributor Security Guidelines

1. **Never Commit Secrets**: Never commit real database passwords, JWT secrets, OAuth tokens, or production API keys. Always use `.env.example` templates and keep actual `.env` files ignored.
2. **Input Validation**: All incoming requests to backend routes must be validated using Zod schemas before touching business logic.
3. **CORS Restrictions**: Do not set permissive wildcard (`*`) CORS origins in production environments.
4. **Server-Side Authorization**: Frontend UI hiding is not security. All state changes, event approvals, and registrations must be authorized and validated on the backend.
