# Security policy

## Reporting a vulnerability

Report suspected vulnerabilities privately through GitHub:
**Security → Report a vulnerability** on this repository
(https://github.com/ACKaraca/solidbuilder/security/advisories/new).

Do not open a public issue for a security problem. Include the affected version or commit, the
platform, a minimal reproduction (a file or command sequence) and the impact you observed.

You can expect an acknowledgment within five working days and a status update at least every
fourteen days until the report is resolved. Fixes are released before details are published, and
reporters are credited unless they ask not to be.

## Scope

SolidBuilder opens files that come from outside parties. The following are always in scope:

- memory-safety faults, crashes or hangs triggered by a crafted input file or network message;
- path traversal, unintended file access or code execution through documents, packages, scripts or
  extensions;
- bypass of an authorization or permission check;
- disclosure of project data to a party that should not receive it.

## Supported versions

Until the first stable release, only the latest tagged release and the `main` branch receive
security fixes. The suite-wide policy is maintained in
[arc-suite](https://github.com/ACKaraca/arc-suite/blob/main/SECURITY.md).
