# Security Policy

## Supported Scope

This repository publishes Agent skills and bundled protocol references.

Security-sensitive issues include:

- A skill instruction that could cause unsafe file access
- A helper script that can damage user files or leak data
- A package that accidentally includes credentials, tokens, local databases, or logs
- A protocol reference that misstates safety or licensing boundaries

## Reporting a Vulnerability

Please do not open a public issue for sensitive reports.

Contact:

```text
yuw4n0@gmail.com
```

Include:

- A short description of the problem
- Affected skill or file path
- Reproduction steps, if applicable
- Potential impact

## Handling

We will review reports as quickly as possible and prioritize fixes that prevent
data loss, secret exposure, or unsafe Agent behavior.
