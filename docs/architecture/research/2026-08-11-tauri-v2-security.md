# Tauri v2 capabilities and permissions security research

- **Status:** RESEARCH
- **Decision status:** Informational only; not an accepted architecture decision
- **Retrieved:** 2026-08-11
- **Provider:** Context7
- **Context7 library:** `/tauri-apps/tauri-docs` (`v2` branch)
- **Scope:** Security properties relevant to a possible future Tauri desktop client and PrepSchool local-lab integration

## Research question

How do Tauri v2 capabilities, permissions, and scopes constrain frontend access to native functionality, and which parts matter to PrepSchool's future local-lab security model?

## Method and limits

Context7 resolved “Tauri v2” to the official Tauri documentation source, `/tauri-apps/tauri-docs`. Two focused documentation queries covered the ACL model and practical filesystem, shell, HTTP, sidecar, remote-origin, and CSP examples.

This record summarizes the returned excerpts; it does not preserve the complete raw Context7 response. It is research, not approval to adopt Tauri. Claims about PrepSchool below are design implications inferred from the cited Tauri material and the current PrepSchool product constraints.

## Key findings

### 1. Tauri v2 uses permissions, scopes, and capabilities

Tauri v2 replaced the v1 core-only allowlist with a unified access-control model:

- **Permissions** enable or deny individual Tauri core, plugin, or application commands.
- **Scopes** constrain resources or parameters available to permitted commands.
- **Capabilities** attach permissions and scopes to selected windows and webviews.

Core permission identifiers use the `core:` prefix. Plugins can provide grouped `default` permissions, but applications can extend or reduce those grants.

### 2. IPC authorization includes the caller context

Tauri's IPC dispatch validates an injected invoke key and resolves access using the requested command, window label, webview label, and local or remote origin. An invocation without matching ACL authority is rejected before the command runs.

Remote content does not receive access to custom commands unless an explicit remote capability grants it. This makes local-versus-remote origin separation a material part of the threat model, not merely a navigation concern.

### 3. Scopes require enforcement by the command implementation

Tauri attaches the configured scope to an authorized invocation, but the command implementation is responsible for interpreting and enforcing it. Therefore, a capability manifest does not compensate for a native command that ignores its scope or accepts unsafe parameters.

The ACL controls whether the frontend can reach a native command. It is not an OS sandbox and does not replace application-level authorization, resource isolation, or validation inside the local service.

### 4. Official examples demonstrate resource-specific narrowing

The retrieved documentation includes these patterns:

- Filesystem access limited to selected resource paths, with explicit allow and deny patterns.
- HTTP access limited by URL patterns, including separate allow and deny entries.
- Shell execution limited to a named executable or sidecar and constrained arguments.
- Capabilities bound to a named window.
- CSP restricted to known content, connection, image, font, and style sources.

These are useful primitives, but a broadly scoped path, URL wildcard, executable, or argument rule remains broad authority even when represented as a valid Tauri scope.

### 5. Shell and sidecar access are especially sensitive

The shell plugin can allow execution of an exact sidecar or command with argument constraints. A generic shell such as `sh -c`, an arbitrary executable path, or weak string validation would transfer substantial host authority to a compromised webview. Tauri permission checks do not make the resulting command string intrinsically safe.

### 6. CSP is complementary defense

Tauri supports a restrictive Content Security Policy for frontend resources and network destinations. CSP can reduce frontend compromise and exfiltration paths, but it does not replace capabilities, backend authorization, or process/container isolation.

## PrepSchool implications

These are recommendations for later architecture work, not accepted decisions.

### Future agent execution boundary

Tauri capabilities are primarily a security boundary for desktop webview-to-native IPC. They can narrow which desktop surfaces may invoke local commands, but they should not become PrepSchool's authority for provisioning arbitrary whole servers or granting unrestricted remote shell access.

Future agent execution environments may include containers, VMs, GPU hosts, or dedicated server workspaces. A Tauri client may let a user request and approve an environment, observe its provisioning progress and resource usage, start or stop it, or connect to it. This lets the desktop expose the workflow without owning the underlying authorization policy.

If PrepSchool introduces remote agent environments, a separate PrepSchool agent/lab control plane should eventually own authorization, provisioning, and resource lifecycle. Its policy model should include:

- capability grants;
- resource quotas;
- time and lease limits;
- session ownership;
- audit logging;
- explicit lifecycle state, including provisioning and readiness so deployment time is visible; and
- human approval for privileged operations.

Tauri would remain one client-side enforcement layer: it may constrain which control-plane operations a desktop surface can request, while the control plane independently authorizes and records the operation. This is a future architectural recommendation only. It does not select an implementation, cloud provider, or orchestration platform.

1. **Use least-privilege capability roles.** If PrepSchool adopts Tauri, separate candidate workspace, lab configuration, playback, authentication/update, and privileged administration surfaces by exact window/webview labels rather than sharing one broad default capability.
2. **Keep privileged webviews local.** Do not load remote content into a webview that can invoke local-lab commands. Any remote capability should be exceptional, origin-specific, and minimal.
3. **Expose semantic commands, not host primitives.** Prefer typed operations such as requesting a session environment or collecting scoped telemetry over arbitrary shell, process, filesystem, Docker socket, or HTTP access.
4. **Keep policy outside the desktop client.** A local-lab agent should independently authenticate local requests and enforce session ownership, allowed operation, phase, path, process, network, and resource limits. A future remote control plane should provide the corresponding authority for remote environments. Tauri ACL should only narrow which requests the UI can make.
5. **Preserve server-controlled interview integrity.** Clock and phase transitions, hint eligibility, solution locking, score changes, and sandbox policy must remain in shared trusted application/domain logic rather than capability configuration or frontend state.
6. **Scope filesystem access per session.** Restrict access to the active workspace, explicitly imported repositories, packaged read-only resources, and consented ephemeral recording locations. Exclude credentials, harness authentication, unrelated repositories, and user configuration by default.
7. **Avoid general-purpose shell execution.** Prefer a bundled, versioned sidecar with structured requests. Avoid `sh -c`, PowerShell command strings, arbitrary executable paths, and permissive argument regexes.
8. **Restrict network destinations.** Limit HTTP/WSS authority to required PrepSchool endpoints and authenticated loopback services. Non-local transport remains HTTPS/WSS as required by current product decisions.
9. **Separate sensitive device permissions.** Microphone, camera, raw recording, and telemetry should have distinct consent and authorization paths. Raw audio/video remains local and ephemeral unless recording is explicitly enabled.
10. **Test the manifest and native enforcement together.** CI should verify expected allow/deny behavior and test that native commands reject out-of-scope resources and malformed parameters.

## Open questions

1. Will PrepSchool use Tauri for a desktop client, or keep browser and TUI clients with a separately installed local agent?
2. Which operations truly need to cross from a desktop webview into the local lab, and what typed command schemas should represent them?
3. Can the local agent use a narrow local transport without exposing a general loopback HTTP service?
4. What OS-level sandboxing is required per platform for Docker, Git, shell, GPU telemetry, model runtimes, and candidate code?
5. How should capabilities differ across Windows, macOS, and Linux without creating inconsistent security guarantees?
6. How are per-session credentials issued, rotated, revoked, and kept outside repositories and frontend storage?
7. How should imported repositories and user-selected paths be represented in scopes without granting parent-directory access or introducing symlink escapes?
8. Which official plugin `default` permission sets are acceptable, and should PrepSchool avoid defaults in favor of explicit command permissions?
9. What automated checks will detect capability drift when Tauri or plugins are upgraded?
10. Should remote documentation be opened in the system browser rather than any application webview?
11. Where should the trust boundary fall between local-lab agents and a future remote agent/lab control plane?
12. How should clients present provisioning time, readiness, quota and lease status, resource usage, and failures without becoming authoritative for those states?
13. Which privileged lifecycle operations require human approval, and how should that approval be bound to a user, session, and audit record?

## Status and next step

**RESEARCH — no architecture choice has been accepted.** Before an ADR, prototype the minimum local-agent command surface and threat-model compromised frontend content, malicious imported repositories, untrusted candidate code, local cross-user access, and sidecar compromise.

## Retrieved source documents

All sources below were returned through Context7 library `/tauri-apps/tauri-docs` on 2026-08-11:

1. [Capabilities](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/security/capabilities.mdx)
2. [Tauri 2.0: permissions, scopes, and capabilities](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/blog/tauri-2.0.mdx)
3. [Tauri v2 beta permissions overview](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/blog/tauri-2-0-0-beta.md)
4. [Tauri v2 release-candidate core permission migration](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/blog/tauri-2-0-0-release-candidate.mdx)
5. [IPC dispatch and ACL enforcement source excerpt](https://github.com/tauri-apps/tauri-docs/blob/v2/packages/tauri/crates/tauri/src/webview/mod.rs)
6. [Content Security Policy](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/security/csp.mdx)
7. [Shell plugin](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/plugin/shell.mdx)
8. [HTTP client plugin](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/plugin/http-client.mdx)
9. [Resource filesystem scope example](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/develop/resources.mdx)
10. [Node.js sidecar example](https://github.com/tauri-apps/tauri-docs/blob/v2/src/content/docs/learn/sidecar-nodejs.mdx)
