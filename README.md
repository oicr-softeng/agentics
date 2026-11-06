# softeng/agentics

A template for structured AI agent collaboration: add it to a project (a team decision) or to your own global context (just for you), so an agent behaves consistently and remembers what it's already learned.

**What this is:** markdown files (`AGENTS.md`, a `.dev/` directory and, where it helps, `DEVELOPMENT.md`) added to a project, or to your own global context, containing structured instructions for how an AI coding agent should behave and what it should remember. The detailed conventions stay in this repository, and the agent reads them from it rather than from a copy. Notes marked "For Claude" give the Claude Code detail; other agents use their own equivalent, unless a note says otherwise.

**What it does:** makes an agent behave consistently across sessions (ask before big decisions, push back on weak ideas, don't repeat itself) and persist project-specific knowledge (open work, known issues, past decisions) in files that outlive any single conversation.

**What it lets you do:** stop re-explaining the same preferences and context every session. Catch a mistake once, not repeatedly across different sessions. Share these conventions with your whole team by committing them to the project, or keep them personal by setting them up only in your own global context.

**Setting it up:** give your agent this repository's address and ask it to set up your own preferences and, for each project that wants it, the project. Agentics itself needs nothing installed or downloaded; for Claude Code, the optional credential guard copied by the second request needs Python, and setup skips it where Python is missing. [Quick start](#quick-start) has the two requests to make.

---

## Terms

| Term | What it is | Example here |
|---|---|---|
| `.dev/` | A directory committed in each adopting project. It holds the project's planned work, known issues, and a dated log of each contributor's sessions, read by people and agents alike. | This repository's own [`.dev/tech-debt.md`](.dev/tech-debt.md) |
| Breaking entry | A `CHANGELOG.md` entry marked `breaking: yes`. A project moving its pin past the entry's release takes the action named by its **Migration:** step. | `committed-env-templates-are-readable` in 0.21.0: the credential hook is copied at adoption, so a project gets the change only by upgrading |
| Convention | One rule file in `template/conventions/`. The agent reads it when its task matches. | `template/conventions/testing.md`, read when writing or reviewing tests |
| Dispatch table | The list in agentics' `template/AGENTS.md` that tells the agent which convention file to read for each task. | The line "Writing or reviewing tests -> read `conventions/testing.md`" in [`template/AGENTS.md`](template/AGENTS.md) |
| Global context | Your agent's own configuration for every project you open, kept outside any repository. | For Claude: `~/.claude/CLAUDE.md` |
| Global-guideline material | Files that stay in agentics. The agent reads them live, or you use them once to set up your global context, and they are never copied into a project. | `template/conventions/`, `template/AGENTS.roles/`, `template/AGENTS.softeng.md`, `template/AGENTS.overture.md` |
| Live pointer | A path in agentics' `template/AGENTS.md` naming a file in agentics. The agent reads that file fresh each time, at the project's pin, from a local clone or the repository's address, rather than from a copy. | `conventions/session-discipline.md`, read from `template/conventions/session-discipline.md` in agentics |
| Pin | The version tag on the first line of a project's `AGENTS.md`. The agent reads agentics at that release's release tag, so a later release changes nothing in the project until the pin moves. A pin of `main` follows the newest commit instead. | `<!-- agentics-template-version: X.Y.Z -->` |
| Project memory | Your agent's own notes about one project, kept in its global context directory and never in the repository. Unlike `.dev/`, it is not committed. | For Claude: `~/.claude/projects/<encoded-project-path>/memory/` |
| Release tag | The git tag on an agentics release's commit. A project pinned to that release has its agent read agentics there. Releases from before tagging began carry none. | `vX.Y.Z` |
| Upstream check | The agent's comparison of a project's pin against agentics' `CHANGELOG.md`, listing the releases newer than the pin. | [`template/conventions/upstream-check.md`](template/conventions/upstream-check.md), run at session start when `propagation_suggestions: yes` is in your global context |
| Version tag | An HTML comment on the first line of a file adopted from agentics, naming a release. On a project's `AGENTS.md` it is the pin; on a copy in your global context, such as a role file, it names the release copied. | `<!-- agentics-template-version: X.Y.Z -->` |

---

## Quick start

**You need an agent that works with files on your computer, such as Claude Code, and this repository's address.** Put the address of the repository you are reading this in where each request below says `<address>`, since that is a copy you can open. Your agent records it and reads agentics from there from then on. Only people contributing to agentics itself keep a copy of it; see [`CONTRIBUTING.md`](CONTRIBUTING.md).

**For the agent given this address:** read each file's exact text rather than a summary, from `https://raw.githubusercontent.com/<owner>/<repo>/main/<path>` for a public repository or through a read authenticated as the user for a private one, and ask nothing technical of the user. Start the first request from [`template/global-context/README.md`](template/global-context/README.md) § Bootstrapping without a project, and the second from [`template/conventions/initialization.md`](template/conventions/initialization.md).

Agentics is fundamentally a personal, global thing: your own behavioural conventions, following you into every project you touch. Adopting it for a specific project doesn't replace that. It adds two things on top of it: a shared, committed record of the project's work in `.dev/`, and any project-specific overrides. Do step 1 first, always; step 2 is optional, and per project.

**Step 1, do this first, always recommended:**

> "Set up my personal AI collaboration preferences using agentics at `<address>`, global context only. Read agentics' files as exact text from their raw address, not as a summary."

Bootstraps your role, writing style, and interaction conventions into your own global context. Nothing gets created or committed in any project. Every project you touch benefits from this foundation, whether or not that specific project has adopted agentics itself.

**Step 2, optional, a decision for a specific project, not just you:**

> "Set up my AI collaboration environment for this project using the agentics template at `<address>`. Read agentics' files as exact text from their raw address, not as a summary."

Make this request in a session opened on the project you want it for, not one opened somewhere else.

This doesn't duplicate step 1 from scratch: it adds the project's shared record in `.dev/` (`roadmap.md`, `tech-debt.md`, `sessions/`) and any project-specific convention overrides on top. It still creates an `AGENTS.md`, which pins an agentics release, keeps a copy of the critical constraints, and has the agent read the rest from agentics at that release. It is not a pointer back to your own global context, so a teammate who hasn't done step 1 personally still gets the same consistent behaviour working in this repo, once their agent has asked them for an agentics address. That redundancy is intentional, not wasted effort. It also creates `DEVELOPMENT.md`, unless your role is general or the project already has a setup guide, and a `CLAUDE.md` stub (for Claude: Claude Code loads that file automatically; for other agents: not needed). These files are meant to be committed and used by every future session, human or agent, that works in this repo.

**What your agent may ask, for step 2, skipping any question already answered in your global context:**

1. What role best describes your work: developer, bioinformatician, AI engineering, or general (non-code work)
2. Whether you are on the softeng team (applies supplementary team conventions if yes)
3. Whether this is an Overture project (applies supplementary product-family conventions if yes)
4. Whether you already have agent conventions for this project (merges rather than replaces)
5. Whether you'd like it to suggest when patterns could be reused across projects
6. Whether you'd like `.dev/roadmap.md` to split into a terse summary plus deeper detail filed separately
7. If your agent can message other sessions (for Claude: `ListAgents`/`SendMessage`), whether to use a shared, cross-project agent index

It stores your answers in project memory (or your global context, for the propagation-suggestions and agent-index defaults) and does not ask again, except that upgrading the project's adoption asks any question added since.

**Once it's done, verify independently, don't just trust the summary.** Look at the project's changed files yourself, in whatever tool you use with it, or ask your agent to list them. You should see `AGENTS.md` and `.dev/` as new, `DEVELOPMENT.md` too unless your role is general or the project already has a setup guide, and `.gitignore` as new or modified. For Claude, also expect `CLAUDE.md` and, unless the hook is already in your global settings, `.claude/settings.json`; for other agents, neither. If `conventions/`, `AGENTS.roles/`, `AGENTS.softeng.md`, or `AGENTS.overture.md` show up too, that's a bug. They're global-guideline material and should never be copied into a project (see [What gets installed](#what-gets-installed)); ask your agent to remove them and re-read `template/conventions/convention-levels.md` § How much to keep locally. Once it looks right, commit those specific files, or ask your agent to.

**One adoption, traced.** A developer has adopted agentics for `billing-api`, a service repository their team works in, through the two requests and the check of changed files. What happens afterwards:

1. At the start of the next session in `billing-api`, the agent reads `AGENTS.md`, which sends it to agentics' `template/AGENTS.md` at the pinned release. That file's "Starting a session" line sends it to `session-discipline.md` at the same release, and from there to the `.dev/` files before any other work.
2. Agentics later publishes a release containing a breaking entry. Nothing changes in `billing-api` yet, since its agent goes on reading the pinned release.
3. With `propagation_suggestions: yes` in their global context, the agent's upstream check at session start lists the entries in releases newer than the pin and proposes moving the pin, with the breaking entry's migration step.
4. The developer approves the proposal, and the agent applies the migration step and moves the pin to that release.

**One thing worth setting expectations on:** this doesn't change what your agent can do the moment you run it. It's where your team puts what it has already learned, so the next session doesn't start from zero. The difference shows up over several sessions of actual use, not on first install; judging it by whether it feels different immediately is the wrong test.

**The other expectation to set: this is pre-release software and is versioned that way on purpose.** It sits at `0.y.z` and will until productization is explicitly decided, which means **breaking changes are routine rather than exceptional**. In release 0.21.0, 48 of its 89 entries were marked breaking. Breaking here means an adopting project needs to take an action when it moves its pin past that release, usually re-copying a file or editing its own `AGENTS.md`, and it is tracked separately from the version bump rather than driving it.

What that asks of you is small and worth knowing before adopting. Your project's `AGENTS.md` pins a release, and your agent reads agentics' rules and conventions at that release, so nothing changes under your project until you move the pin. When you want to move, ask your agent to upgrade the project: it goes through the entries marked breaking with you, applies their migration steps once you approve, and changes the pin. [Keeping up to date](#keeping-up-to-date) has the details.

Known gaps in agentics are tracked in this repository's own [`.dev/tech-debt.md`](.dev/tech-debt.md) rather than discovered by adopters, which is the practice you should expect from anything calling itself pre-release.

---

## What gets installed

```
AGENTS.md                   the pin, a copy of the critical constraints, project-specific content; agentics for the rest
CLAUDE.md                   stub: Claude Code loads it automatically, it points at AGENTS.md for everything
DEVELOPMENT.md              human developer setup and onboarding guide (fill in per project; skipped for the general role or an existing setup guide)
.dev/
  roadmap.md                planned work across features and phases
  tech-debt.md              known issues with standalone/blocked tags
  sessions/                 one file per contributor per day, dated log of decisions and open threads
```

What this actually looks like in practice, a real entry from this repo's own `tech-debt.md`:

> OWASP A08-A10 in security-guidelines.md not validated against team review

That's it: a fact about the system, the same kind of line found in any mature codebase's issues list or TODOs, just structured for an agent to read reliably too.

Where each part of this repository ends up:

```
CHANGELOG.md                every change, its version, and whether it is breaking
CONTRIBUTING.md             how to propose a change or add a role file
docs/                       security guides and the design philosophy, read in place
scripts/                    checks run from a copy of agentics, never copied into a project
template/
  .claude/settings.json     for Claude: copied per project, or once into global settings; for other agents: not used
  AGENTS.md                 read live at your project's pin; never copied into a project
  AGENTS.overture.md        read live, or used once to set up your global context; never copied into a project
  AGENTS.project.md         copied into your project as AGENTS.md
  AGENTS.roles/             read live, or used once to set up your global context; never copied into a project
  AGENTS.softeng.md         read live, or used once to set up your global context; never copied into a project
  CLAUDE.md                 for Claude: copied into your project; for other agents: not needed
  conventions/              read live from agentics, never copied
  DEVELOPMENT.md            copied into your project unless it has a setup guide or your role is general, then filled in
  global-context/           used once to set up your global context, never copied into a project
testing/                    for agentics contributors only, never copied
```

If your agent's global context doesn't yet define your role, your team's conventions, or your product family's, bootstrap it once from the matching role, team, or product-family file, the same way `template/global-context/` templates get copied into your global context directory (for Claude: `~/.claude/`). That is a one-time step, not a per-project one.

For Claude: `template/.claude/settings.json` is a hook that denies tool calls naming a credential file; copy it into each project, or add it once to your global `~/.claude/settings.json` instead. For other agents: the hook does not apply, and [`docs/security-for-developers.md`](docs/security-for-developers.md) says what to build in its place.

---

## Scripts you can run yourself

Nothing in [`scripts/`](scripts/) is copied into your project. Run it from a copy of this repository, the same way `conventions/` stays a live pointer rather than a copy: a checker that gets copied goes stale silently and then reports "ok" about rules that have since changed.

### `check-agent-index.sh`

```
./scripts/check-agent-index.sh              # reads ~/.claude/agent-index.md
./scripts/check-agent-index.sh <some-path>  # or any file you point it at
```

**Skip this entirely unless you run several agent sessions at once across different projects.** It inspects the optional agent index, which records which agent owns which repositories or subtrees. If you work with one agent in one project, there is nothing here for you, and having no index at all is a perfectly normal state, which the script says when it finds none.

If you do, it answers a question that turns out to be hard to answer by reading: **who owns what, and what does that actually resolve to.** Ownership can be a subtree, so a monorepo may have several owners, and a path belongs to whoever holds the longest matching prefix. That is easy to get wrong by eye once entries nest. It prints the resolution tree explicitly, flags an entry claiming the whole of a top-level space, such as an organization, without declaring that space its own (a claim that quietly makes its holder the owner of everything beneath it), and reports duplicate paths, machine-specific absolute paths, and entries still on an older schema. It never edits anything.

**It also prints what it cannot know, on purpose.** Whether any session is currently holding a given label, whether a session that holds one knows that it does, and which sessions are live. Those live in the sessions themselves rather than in a file, because an agent's identity is assigned by you rather than inferred from its surroundings, so nothing on disk can answer them. A report that let you read the registry as a complete census of your running agents would be actively misleading, which is the whole reason the limitation is in the output rather than in a footnote here.

Exits non-zero when something is genuinely wrong, so you can wire it into a hook if you want to.

`testing/` is a different thing and is for people contributing to agentics itself, not for adopters; see [`CONTRIBUTING.md`](CONTRIBUTING.md).

---

## Adopting by hand

To adopt a project without the step 2 prompt, follow [`template/README.md`](template/README.md) § How to adopt, which names the few files in [`template/`](template/) to copy into your project root and the ones that stay in agentics.

---

## The illusion of context

When people say an agent "understands the codebase," they rarely mean it read every file. They mean something narrower, and far more fragile.

It read the right file, at the right moment, before that moment passed.
It remembered why the last attempt at this was rejected.
It knew which rule was mechanical, and which one still needed judgment.

None of that came from reading more. It came from someone having already written it down, in a place built to be read again.

That's not understanding. It's not memorization either.

**It's context that survived past the session that created it.**
Agentics doesn't teach an agent to understand your system: it gives your team a place to put what it already knows, so the next session doesn't have to earn it again and again.

This template goes further, though: its conventions also shape how the agent reasons and collaborates, not just what it can parse, a critical aspect other similar tools don't cover. In essence, an agent following it "questions" ideas instead of simply executing them blindly, whenever possible surfaces better alternatives, and checks that a proposed approach actually serves the stated goal before committing to one.

---

## Design

- **Dispatch, not dump**: `AGENTS.md` stays lean; convention detail lives in separate files loaded only when relevant. [Two tiers](#two-tiers-how-often-something-is-needed-not-just-how-much) explains what decides whether something is copied into a project or left as a live pointer.
- **Agent-neutral**: the template works with Claude, Codex, Copilot, and others. Role files, convention files, and `AGENTS.md` are written to describe your agent's global context directory in general terms, with a Claude-specific path given as an example beside it where needed.
- **Additive**: roles and org layers (softeng) add to the base; they do not replace it. Merging with your existing setup is always the right choice.
- **Contribution ladder**: good practices discovered in a project can bubble up: project memory → agent global context → PR to agentics.

### Two tiers: how often something is needed, not just how much

Only what would fail silently if read live, on demand, is copied into the adopting project. No convention file ever is. Everything else stays a pointer to agentics, whether that's a local clone or a remote URL.

**Needed every session, with no natural trigger of its own:** the instruction to read agentics' `template/AGENTS.md`, whose dispatch table holds the instruction to read the session-start checklist (never the checklist itself), and the project's own pin. Nothing else prompts checking these, so if the check doesn't fire reliably, drift goes unnoticed. This was tested directly: a citation ("the base convention lives in X") did not reliably cause a fresh read of the current file; only an explicit instruction ("read X now, every session") did. One section is copied for a different reason: § Critical constraints, so that inside a project with the pointer file the constraints hold even when agentics cannot be read.

**Needed only when actually doing that task:** writing tests, reviewing a PR, security-relevant work, writing docs. These already have a strong trigger: the agent is doing that task right now. A live dispatch pointer works fine here, which is why `AGENTS.md` can stay lean for all of them.

**Single source, not two copies:** agentics' `template/AGENTS.md` holds the canonical dispatch table, read at each project's pin; a project's `AGENTS.md` and `CLAUDE.md` point at it rather than keeping their own. `AGENTS.md` dispatches to convention files rather than inlining them, because its consumers (Cursor, Copilot, Aider) have file-system access like any other coding agent, and the AGENTS.md standard's own guidance recommends fetching on demand. One dispatch table, referenced from both files, removes a class of drift rather than managing it.

**Local clone versus remote URL is a separate axis.** It changes where a needed convention is fetched from, not whether it needs fetching at all. A contributor with agentics cloned locally and a new adopter working from a GitHub URL alone should both end up doing the same thing: reading what is needed every session live, from whichever source is actually available to them.

## Keeping up to date

**Your project's `AGENTS.md` opens with its pin, `<!-- agentics-template-version: X.Y.Z -->`**, set at adoption to the newest release in this repo's `CHANGELOG.md`. Each release commit carries the release tag `vX.Y.Z`, and your agent reads `template/AGENTS.md` and every convention at that tag, so a release reaches your project only when you move the pin. To move it, ask your agent to upgrade the project's agentics adoption: it reads the entries since your pin, applies the migration steps of those marked breaking once you approve them, re-copies § Critical constraints, and changes the pin, per [`template/conventions/upgrading-adoption.md`](template/conventions/upgrading-adoption.md). A pin of `main` follows the newest commit instead, for trying conventions not yet released; for a contributor it reads the working tree of their clone. Whatever you opted into, your agent tells you at session start of any release newer than your pin that fixes a security weakness.

Only a release with a release tag can be pinned. A project adopted before releases were tagged holds a full copy of `template/AGENTS.md` and reads conventions from the newest commit until it migrates to the pin, which the same upgrade procedure does once.

## Security

Running an agent with real access to your repos has a threat model of its own, separate from the application security your code already needs.

- [`docs/security-for-developers.md`](docs/security-for-developers.md): **written for you, not your agent.** What to verify yourself rather than take on your agent's report, which agent claims are unreliable and how to check each cheaply, the trust boundaries held only by a person (pull request text is attacker-controlled, a peer session's message is not your approval, your global context is loaded everywhere and usually not version controlled), and what to do once something has gone wrong. Start here.
- [`docs/agent-security.md`](docs/agent-security.md): the threat model itself, the attack vectors, the session-start integrity check agents run, and an honest account of what cannot be caught automatically. Dual audience.

The template ships a credential-file blocklist hook, but it is a speed bump on the routine accident rather than a boundary, and it is specific to Claude Code. `security-for-developers.md` states its limits plainly and gives you a short command to confirm it is actually live, which is worth running: this repo shipped that hook in a state where it silently allowed everything, and a control that fails by allowing produces no error, so only a direct test reveals it.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose changes and how to create new role files.

If you discover a convention in your project that could benefit other teams, open a PR. If you turned on propagation suggestions during setup, your agent flags likely candidates as they surface.

Building something similar for your own team? [`docs/deterministic-by-design.md`](docs/deterministic-by-design.md) is the design philosophy behind how this repo tries to make its own conventions reliable: which parts should be a mechanical check wired into something that always runs, which parts need an explicit override instead of a plain instruction, and which parts should be made structurally impossible rather than caught after the fact.
