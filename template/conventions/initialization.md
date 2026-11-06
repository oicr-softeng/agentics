# Initialization

Read this when setting up agentics in a project for the first time, or in the user's global context alone.

## Recording where agentics lives

**Do this first, in project setup and global-only setup alike, unless your global context already has an agentics entry.** Every later read of a convention resolves against it, per agentics' `template/AGENTS.md` § When to read what.

- **`Repo URL`: the address of the agentics repository the user pointed you at**, taken from their request, or asked for if it gave none. There is more than one, such as a team's private repository and a public one, so never substitute one you know of: the one they gave is the one they can open. Try reading an address before recording it. If a later request gives a different address from the recorded one, keep whichever you can read, and ask only if both read.
- **`Path`: only where they keep a local clone**, which agentics contributors do and nobody else needs. Never ask anyone to make one.
- **Write it as an `agentics` entry in your global context's cross-project map** (for Claude: `~/.claude/projects.md`, created from `global-context/projects.md` if absent, keeping its guidance and dropping its bracketed placeholder entries), then tell the user in one line what you recorded and where. Ask the team question from § Setting up a project first, unless it is answered, since `Owner:` depends on it: `Owner:` names the user's team where that team maintains the repository, and is `external` otherwise, a user with no team included.

**Not everyone who adopts agentics is a developer, so every technical step is yours.** Never ask the user to clone, install or configure anything. Agentics' `template/AGENTS.md` § When to read what says how to read a file's exact text from a public or a private repository; if neither works, say so and ask whether they can give you the address of a public copy, rather than continuing without the conventions.

## Setting up a project

If no project memory exists for you in this project yet:
1. Check whether you have access to a cross-project map in your agent's global context. If yes, read it for cross-project relationships. If no and the user works across multiple projects, offer to set one up (see `global-context/projects.md` in the agentics template for the recommended format).
2. Ask: "What best describes your primary work on this project?": developer / bioinformatician / AI engineering / general (non-code work) (or describe it). If the answer is already in your global context, skip this question. Otherwise read the role file named beside the answer in the table under **Where each answer is recorded**.
3. Ask: "Are you part of the softeng team?": if yes, apply conventions from `AGENTS.softeng.md` on top of your role conventions. Skip if already known from global context.
4. Ask: "Is this an Overture project?": if yes, apply conventions from `AGENTS.overture.md` on top of your role conventions. Skip if already known from global context.
5. Ask: "Do you already have agent conventions for this project?": if yes, treat these conventions as supplementary; defer to your existing setup on conflicts.
6. Ask: "Whatever you answer here, in a project pinned to an agentics release I tell you at session start about any newer release that fixes a security weakness. Would you like me also to tell you about every other new release, and to suggest when a convention could be useful beyond the project where it came up?": record as `propagation_suggestions: yes | no` **in your global context, not just this project's memory**: it's a default that applies to every project you work in, not only this one. Skip if already known from global context.
7. Ask: "Would you like `.dev/roadmap.md` to split into two layers? A short, human-scannable roadmap you can read directly, with each entry's deeper reasoning, alternatives, and history living in `.dev/docs/atlas/roadmap/<topic>.md` instead, cross-linked from the roadmap entry. If no, `roadmap.md` keeps its current density.": the default is no. Record as `roadmap_split: yes | no` **in this project's own memory, not global context**: this is a per-project stylistic choice about how this specific roadmap gets read, not a default across projects. Skip if already known from project memory.
8. If your agent has cross-session messaging (for Claude: `ListAgents`/`SendMessage`): ask "Would you like to register in a shared, cross-project agent index, so sessions in different projects or workspaces can reach each other directly?": record as `agent_index: yes | no` **in your global context, not just this project's memory**: same reasoning as `propagation_suggestions` above. If yes, record `agent_index: yes`; that opts this project into using the index, and is not itself a registration. **Registering is a separate act that happens only when the user confers a label**, per `conventions/agent-index.md` § Registering and changing ownership, and most sessions never register at all. Skip entirely if your agent has no cross-session messaging capability, or if already known from global context.

**Where each answer is recorded.** Project memory means a file named `agentics-setup.md` in this project's memory, wherever agentics' `template/AGENTS.md` § Memory and contribution hygiene puts it (never in this repository), listed in your agent's memory index where it keeps one (for Claude: `MEMORY.md`). Global context means a list headed `## agentics setup` in your agent's global context file (for Claude: `~/.claude/CLAUDE.md`). Both hold one `key: value` line per answer, such as `role: developer`. Do not ask again once an answer is recorded.

| Answer | Key and values | Project setup | Global-only setup |
|---|---|---|---|
| Role | `role`: `developer` (role file `AGENTS.roles/dev.md`), `bioinformatician` (`AGENTS.roles/bio.md`), `ai-engineering` (`AGENTS.roles/ai-eng.md`) or `general` (`AGENTS.roles/general.md`) | project memory | global context |
| softeng team | `softeng_team`: `yes` or `no` | project memory | global context |
| Overture | `overture_project`: `yes` or `no` | project memory | global context |
| Existing setup | `existing_setup`: `yes` or `no` | project memory | not asked |
| Roadmap split | `roadmap_split`: `yes` or `no` | project memory | not asked |
| Propagation suggestions and notices of other releases | `propagation_suggestions`: `yes` or `no` | global context | global context |
| Agent index | `agent_index`: `yes` or `no` | global context | not asked |

A project can override a global answer later in its own `agentics-setup.md`; when both exist, the project's record wins for that project only.

Then finish the setup:

1. Choose the release to pin: the newest under agentics' `CHANGELOG.md` § Released, confirmed by reading `template/AGENTS.md` at its tag. Where that read fails and a read at `main` succeeds, the release has no tag: pin the newest release whose tag reads instead, and tell the user. If no release's tag reads, tell the user and ask whether to pin `main`.
2. Create the project's files as agentics' `template/README.md` § How to adopt lists them, each read at the pinned release, with the pin on the first line of `AGENTS.md`, and `DEVELOPMENT.md` left out where that list says.
3. Add the project to your global context's cross-project map: its name under § Agentic projects, and an entry under § Projects in the format given there.
4. Record the adoption in a session file in `.dev/sessions/`, per `session-discipline.md` § Session file identity and `entry-formats.md` § Session file entry format, so the files it created are attributed at the next session start and `.dev/sessions/` is part of the adoption commit.
5. Make sure the commit can succeed. If the project folder is not a git repository, ask in plain words whether to make it one, so its history is kept, and create it with `git init` if the user agrees. If `git config user.name` or `git config user.email` prints nothing, ask the user what name and email to sign their work with, and set them for this repository only, without `--global`. Never invent an identity.
6. Offer to commit the new files.

**Acted on once, with one exception: an upgrade re-opens these questions.** The dispatch line pointing here says adoption runs this once, which is true of adoption and false of the whole lifetime. A question added to this file after a project was set up is never asked of that project, because the run condition is that no project memory exists yet and by then it does. `upgrading-adoption.md` exists partly to re-ask exactly those, so the two statements are both true and read as contradictory unless this one says so.
