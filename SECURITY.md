# Security policy

## Reporting a vulnerability

Report it privately, through **Report a vulnerability** on this repository's **Security** tab. Do not open a public issue or pull request, and do not describe it in a discussion: the description is the disclosure, and in public anyone can read it before a fix exists.

Include what it affects and how to reproduce it. The report stays private until a fix is released.

## What counts here

Agentics is mostly prose, so most of what it ships cannot be exploited directly. A vulnerability here is a shipped file or instruction that leaves an adopting project less safe than it would be without it. Examples: a hook or settings file that weakens the agent's permission controls, an instruction that leads an agent to expose a credential, or a convention that tells an agent to publish something it should keep private.

## Which releases receive fixes

Agentics is pre-1.0, and only the latest release receives fixes. Conventions are read live from agentics at the release pinned by a project's `AGENTS.md`, so a fix to one reaches a project only when it moves its pin to a release carrying the fix, or at once for a project pinned to `main`. At every session start, the agent in a pinned project tells the user of any newer release carrying a security fix, whatever the project opted into. Files copied into a project at adoption, such as the copy of § Critical constraints in `AGENTS.md` and the credential hook in `.claude/settings.json`, also change only when that project upgrades. The upgrade procedure in `template/conventions/upgrading-adoption.md` checks the credential hook by behaviour before anything else, because no convention change reaches the copied `.claude/settings.json`.

## How a fix is disclosed

Before 1.0.0, a fix for a vulnerability carried by an earlier release is announced through its release's `security` changelog entries and the session-start notice described above. From 1.0.0, it also ships with a security advisory on this repository, giving the affected and fixed versions, the severity, and a workaround for anyone who cannot upgrade at once. The sequence is in `template/conventions/security.md` § Releasing the fix for a hole carried by a published release.
