# Claude Code settings: what to adopt from a simpler file

**Scope: Claude Code 2.1.292 on macOS, checked against its settings documentation on 2026-10-30.** Every setting name, version minimum and behaviour here belongs to that harness. Another agent has its own permission model, and the sandbox on Linux and WSL2 differs from macOS in where path wildcards work. Read it as a review of one harness's settings, not as how agents are configured in general.

**A settings file from another developer looked much simpler than ours, and the reason is the sandbox.** Most of its protection sits in Claude Code's sandbox, which the operating system enforces on every command and every program that command starts. Ours relies on a `PreToolUse` hook that inspects each command's text. The sandbox is worth adopting. About half the rest of that file would break how agentics works: memory, messages between sessions, and checking facts on the web.

**Nothing here is applied yet.** Every change waits on one of the decisions in § Open decisions.

## Terms

| Term | Meaning here |
| --- | --- |
| Deny rule | A permission rule that blocks a tool call in every mode, with no prompt and no way to approve it in the moment |
| Ask rule | A permission rule that always prompts the developer, even in auto mode, where most calls run without a prompt |
| Protected path | A directory, such as `.git` or `.claude`, whose writes are never approved silently in the default mode. In auto mode those writes go to the classifier instead of the developer |
| Classifier | The model that reviews actions in auto mode in place of a prompt |
| Sandbox | Claude Code's boundary for shell commands, enforced by the operating system (Seatbelt on macOS). It limits which files and hosts a command, and every program it starts, can reach |
| Enforcement, governance | As in [`security-guidelines.md` § Enforcement or governance](../../../template/conventions/security-guidelines.md): enforcement fails closed against someone acting without consent; governance keeps a choice with the person who owns it, and software checks only what it can verify |

## The finding that applies whatever else is adopted

**In auto mode, a write to `~/.claude/` is decided by the classifier, not by the developer.** The protected-paths table in Claude Code's permission-modes documentation says so. It is consistent with an earlier unprompted edit to the developer's global instruction file by another session.

**An ask rule closes this for the file tools.** Ask rules prompt even in auto mode. A deny rule would close it too, but would also refuse the edits the developer asks for.

**The sandbox closes it for shell commands.** Listing the same files in `sandbox.filesystem.denyWrite` stops any shell command from writing them. With both in place, the only way left to edit those files is the one that asks the developer.

## Verdicts, by what each setting defends against

### Adopt in user settings

| Setting | What it defends against | Note |
| --- | --- | --- |
| `sandbox.enabled`, `sandbox.failIfUnavailable`, `sandbox.allowUnsandboxedCommands: false` | A command reaching files or hosts some way no rule anticipated, whether by mistake or because content it read steered it | The boundary holds whatever form the command takes. With the opt-out off, an agent cannot run a command outside the sandbox; the developer still can, with `!` |
| `sandbox.network.allowedDomains` | Data leaving for an unknown host | The compared file allows GitHub only. Ours would also need `*.github.com` for the API and `registry.npmjs.org`. Leave `strictAllowlist` off at first: in auto mode the classifier then judges an unlisted host one command at a time, and the list grows from real use |
| `Read` deny rules for `~/.ssh/**`, `~/.aws/**` and most of the hook's file list | Credential reads | A `Read` deny rule binds the file tools, blocks `Edit` and `Write` on the same path, and is added to the sandbox's read block, so it holds for every program. Leave out files a tool reads by itself, such as the kubeconfig and Docker's config, or `kubectl` and `docker` stop working in the sandbox |
| `Edit` deny rules for `~/.gitconfig` and `.claude/settings.local.json` | An agent setting a git hooks path or credential helper, or widening its own permissions | Neither file is one an agent is asked to change |
| `isolatePeerMachines: true` | A message leaving this machine without the developer's approval | No cost while every session runs on one machine |
| `disableRemoteControl: true` | Someone holding the developer's claude.ai login driving a session on this machine | Stronger than `remoteControlAtStartup: false`, which leaves the switch inside a session |
| `$schema`, with the right URL | Mistyped keys | The compared file points at JSON Schema's own meta-schema, which checks nothing. The right URL is `https://json.schemastore.org/claude-code-settings.json`, which accepts keys it does not know, so its gaps raise no false errors |

### Adopt the intent, not the rule

| Their rule | Why not as written | Instead |
| --- | --- | --- |
| `Edit` deny on `**/.claude/**` and `**/CLAUDE.md` | A deny rule gives the developer no way to say yes. It would block memory, the agent index, the template's own `.claude/settings.json`, and every global edit the developer asks for | An ask rule on the global `CLAUDE.md` and `settings.json`, and the same two paths in `sandbox.filesystem.denyWrite`. The choice stays the developer's, and the software makes sure they are asked: governance, not enforcement |

### Skip

| Setting | Why |
| --- | --- |
| Deny `WebFetch`, `WebSearch`, `curl`, `wget` | `code-style.md` requires checking versions and dated facts live, and this review needed the documentation. The permissions documentation also says a `Bash(curl *)` rule does not match the program by its full path or inside `sh -c`; the network allowlist is what holds |
| Deny `ssh`, `scp`, `rsync`, `nc`, `telnet`, and the HPC scheduler commands | The same gap, and the network allowlist already covers them at the operating-system level. The scheduler commands only matter on an HPC cluster |
| `crossSessionInbound: "refuse"` | Drops every message from the developer's other sessions, which is how owners on the agent index reach each other. `"hold"` is the milder value: a notice is shown and the message is not delivered. A project can set a stricter value for itself |
| `autoMemoryEnabled: false` | Agentics relies on memory |
| `disableAutoMode` | The developer uses auto mode; the ask rules cover the files where a prompt matters |
| `plansDirectory: ".plans"` | Puts plan files inside the repository, where they can be committed |
| `sandbox.filesystem.denyRead: ["/"]` with an `allowRead` list | `permissions.blockReadsOutsideWorkingDirectories` does the same job with a list Claude Code maintains; see decision 3 |
| `enableArtifact`, `disableAgentView`, `sshConfigs`, `theme`, `autoCompactEnabled` | Personal preference, the default, or, for `sshConfigs`, effective only from organization-managed settings |

## Proposed user settings

Additions to `~/.claude/settings.json`, keys in alphabetical order:

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "disableRemoteControl": true,
  "isolatePeerMachines": true,
  "permissions": {
    "ask": ["Edit(~/.claude/CLAUDE.md)", "Edit(~/.claude/settings.json)"],
    "deny": [
      "Edit(**/.claude/settings.local.json)",
      "Edit(~/.gitconfig)",
      "Read(**/*.key)",
      "Read(**/*.pem)",
      "Read(**/*.tfstate)",
      "Read(**/.env)",
      "Read(~/.aws/**)",
      "Read(~/.netrc)",
      "Read(~/.pgpass)",
      "Read(~/.ssh/**)"
    ]
  },
  "sandbox": {
    "allowUnsandboxedCommands": false,
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": { "denyWrite": ["~/.claude/CLAUDE.md", "~/.claude/settings.json"] },
    "network": {
      "allowedDomains": ["*.github.com", "*.githubusercontent.com", "github.com", "registry.npmjs.org"]
    }
  }
}
```

- **The hook stays.** A deny rule cannot say "every `.env.*` except `.env.example`", and the hook can.
- **Git keeps working with `~/.ssh` blocked** because this setup's remotes use HTTPS. A remote over SSH would need its key readable.
- **A test suite that starts a local server needs `sandbox.network.allowLocalBinding: true`.** The default is `false`.

## For the template

Adopting projects receive `template/.claude/settings.json`.

| Candidate | Verdict |
| --- | --- |
| Ask rules on the root `AGENTS.md`, `CLAUDE.md` and `.github/copilot-instructions.md` | Yes. It turns the critical constraint against changing instruction files without instruction into a prompt in every adopting project |
| `isolatePeerMachines: true` | Yes. A project can turn it on but cannot turn it off |
| `$schema` | Yes |
| `Read` deny rules matching the hook's file list | Yes, leaving out files tools read by themselves, as in the user settings |
| `sandbox.enabled` | No. Recommend it in the documentation instead: it adds network prompts for every adopter, at a time when adopters are already leaving over cost |
| `blockReadsOutsideWorkingDirectories` | No. It would cut adopters off from the global-context files in `~/.claude/` |
| `enableArtifact`, `strictAllowlist` | Not possible: Claude Code ignores both in project settings |

## Version minimums

| Setting | Requires Claude Code |
| --- | --- |
| `disableRemoteControl` | 2.1.128 |
| `disableClaudeAiConnectors` | 2.1.182 |
| `enableArtifact` | 2.1.196 |
| `sandbox.network.strictAllowlist` | 2.1.219 |
| `crossSessionInbound`; the approval `isolatePeerMachines` asks for | 2.1.224 |
| `modelSettings` | 2.1.251 |
| `permissions.blockReadsOutsideWorkingDirectories` | 2.1.257 |

## Pitfalls worth knowing when writing one

- **Sandbox paths and permission-rule paths use different prefixes.** In `sandbox.filesystem` lists, `/tmp/x` is absolute. In `Read` and `Edit` rules, `//tmp/x` is absolute and `/x` is relative to the project.
- **A `$schema` URL is easy to get wrong silently.** JSON Schema's own meta-schema validates nothing about Claude Code's keys.
- **`modelSettings` is keyed by a model's canonical name.** An entry matches that model's aliases and dated IDs, not a later model, so an entry for an older Opus does not apply once `"opus"` resolves to a newer one.
- **`blockReadsOutsideWorkingDirectories` already re-opens what a hand-written `allowRead` list usually carries:** the working directories, the session's temp directory, the global git configuration, and the parts of `~/.claude` Claude Code needs. The compared file's list also leaves out `/usr`; whether commands still run without it was not tested.
- **A rule that blocks a command by name does not block the program.** The permissions documentation shows `Bash(curl *)` missing the full path and `sh -c`. Pair such a rule with the sandbox when the restriction has to hold.

## How this was checked

- Each key was looked up in Claude Code's settings, settings-reference, sandboxing, permissions and permission-modes documentation, read on 2026-10-30.
- The public JSON schema lacks four keys the compared file uses (`blockReadsOutsideWorkingDirectories`, `modelSettings`, `isolatePeerMachines`, `crossSessionInbound`), all of which the documentation describes.
- The installed version, 2.1.292, meets every minimum in § Version minimums.
- **Not tested:** running with the sandbox on. The costs named here for `kubectl`, `docker` and local test servers come from the documentation, not from a trial.

## Open decisions

1. **Apply § Proposed user settings to the developer's `~/.claude/settings.json`?** It is the developer's file, so it changes only on their explicit yes. Its credential hook already matches the template's. A sub-choice: also deny sandboxed commands the organization's internal hosts, as the compared file does. That fits deploys going only through Jenkins, but would block `kubectl` debugging if the cluster's API is on one of those hosts.
2. **Set `disableClaudeAiConnectors: true`?** A session connected two claude.ai connectors, a documents service and Atlassian, without asking. Each is a path for data to leave and for outside content to steer the agent. Against the iMicroSeq health-data bar, the recommendation is yes: connect them when a task needs one, not automatically.
3. **Set `permissions.blockReadsOutsideWorkingDirectories`?** It enforces the organization's rule against reading files the developer has not approved, with the workspace's folders as the approval. The cost: the developer's global instruction file reads agentics conventions and `~/.claude/agent-index.md` by absolute path from every project, and those reads fail in any workspace that does not include them. The workaround, listing agentics under `permissions.additionalDirectories`, also makes agentics writable from every session, which is how another session's edits reached agentics before. The recommendation is not yet: revisit once the work on session-start token cost reduces what sessions read from agentics.
4. **Make the changes in § For the template,** unstaged for review, with changelog entries?
