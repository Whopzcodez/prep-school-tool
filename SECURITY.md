# Security Policy

## Reporting a vulnerability

Please report suspected vulnerabilities privately through this repository's GitHub Security Advisories **Report a vulnerability** feature when it is available. Include:

- a concise description and affected component;
- reproduction steps or a proof of concept;
- potential impact;
- any known mitigations; and
- whether sensitive data or credentials may already be exposed.

Do not open a public issue, discussion, or pull request containing exploit details, secrets, private user data, or protected interview material. If private vulnerability reporting is unavailable, use the repository owner's GitHub contact options to request a private reporting channel without disclosing sensitive details publicly.

Maintainers will acknowledge the report, assess severity and scope, coordinate remediation, and disclose the issue after a fix or mitigation is available when appropriate. Please allow a reasonable remediation period before public disclosure.

## Security-sensitive areas

Treat reports involving any of the following as security-sensitive:

- credentials, API keys, access tokens, signing material, or harness authentication;
- private candidate or user data;
- audio or video recordings, including unintended retention or transmission;
- sandbox escape or isolation failure;
- local-lab capability escalation or access outside an approved scope;
- evaluator or protected reference-solution leakage to the live interviewer or candidate;
- unauthorized agent execution, provisioning, connection, or lifecycle operations; and
- secrets accidentally committed to Git, including secrets retained in history.

## Handling exposed secrets

If a secret is committed, do not rely on deleting the file in a later commit. Revoke or rotate the credential immediately, assess usage, and coordinate any required history cleanup. Avoid copying the secret into issues, chat, logs, or pull-request comments.

## Scope and expectations

Security fixes must preserve PrepSchool's server-controlled interview integrity, least-privilege local-lab boundaries, session ownership, and separation between the live interviewer and protected evaluator/reference material. Raw audio and video remain local and ephemeral by default unless recording is explicitly enabled.

This policy does not promise support windows or response times that the project has not formally established.
