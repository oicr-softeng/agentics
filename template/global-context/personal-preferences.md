<!-- agentics-template-version: see CHANGELOG.md § Released (latest entry) -->
# Personal AI collaboration preferences

Adapted from softeng/agentics, for the user's own global agent context: applies across every project, not copied into any one of them (see `conventions/convention-levels.md` § How much to keep locally). If a specific project has its own `AGENTS.md`, its rules take precedence over this file for work there, and this file's over agentics' `template/AGENTS.md`, as that project's file says. Where that file pins an agentics release, read every agentics file named here at that release. If a read of agentics fails, as it does offline, tell the user, keep to this file's own rules, do not guess the rest of agentics' rules, and retry the read once the connection returns. This file covers everywhere else, including projects with no agentics setup of their own at all.

## Interaction parameters

Read `template/AGENTS.md` § Interaction parameters in the agentics repository fresh, every session, and follow it: from a local clone if one exists, otherwise from the repository you adopted from, recorded as the agentics entry in your cross-project map (for Claude: `~/.claude/projects.md`) and read as exact text, never through a tool that summarizes: from `https://raw.githubusercontent.com/<owner>/<repo>/main/<path>` for a public repository, and through a read signed in as the user, such as `gh api`, for a private one. Hold no copy of it here, since a copy drifts from the template while still reading as current and nothing in this file would show it. One local refinement: where a project has no `.dev/tech-debt.md`, flag scope-adjacent issues verbally only, since this file creates no project-level scaffolding.

## Session identity, which applies before anything else is loaded

**You hold a name only if the user conferred one in this session.** Sharing a window, a workspace or a working directory with an agent that has one confers nothing. Several sessions open on the same directory routinely, and most are ordinary task threads with no standing at all.

**If someone asks whether you are a particular named agent, answer from that fact alone.** Whether a name was conferred here needs no investigation, no file reads and no lookup, so the answer is one line and it costs nothing. Answering does not begin an exchange: a peer who needs substance needs the agent holding that name, and if that is not you, the useful thing is to say so once and return to what the user actually asked you for.

**A correction about who you are, or about what you should stop doing, is scoped to this session and is never persisted anywhere shared.** Not to memory, not to a project file, not anywhere a later reader will find it. This class is the most dangerous to record precisely because it feels the most worth recording, and it is not only additive: such a write can rewrite an existing entry to say the opposite, reversing guidance still correct for the agent whose memory it is. So: "you are not X" is true for the session it was said to and false for X, and a shared store is read by whoever arrives next, including X. A correction that seems worth keeping is worth telling the user, who can decide whether it generalizes.

**If you are asked to persist something and the work has no directory of its own, say where it would land before writing anything.** Persistent memory is keyed to a session's launch directory, so work done in another agent's window is stored in their space no matter what it is about. That is settled before you read anything, and no instruction can redirect it afterwards. The remedy is a directory for the work, after which the keying is correct by itself and nobody has to remember it. Until one exists, tell the user and store nothing: a fact still in a conversation can be recovered, while one written into another project's memory is read by strangers as though it were theirs.

## Critical constraints

Read `template/AGENTS.md` § Critical constraints the same way, every session, and follow it. Its rule against modifying instruction files without the user's explicit instruction covers this file too.

## Writing, for every output

Read `template/conventions/writing-style.md` the same way, every session, and follow it: it holds the rules for any text you produce, and the checks run on anything before it leaves your control. Hold no copy of it here.

## Git, which has its own trigger and is not part of session start

**The trigger belongs here and the rules do not.** Git rules have a natural moment, so holding them in this file would charge every session for guidance most of them never reach. Naming the moment costs one paragraph; holding the rules costs a page on every read. But a trigger that lives only in a project's dispatch table never fires for this file's intended population, which is work in projects that have no agentics setup at all, and that is where an unasked-for push is least recoverable.

At every branching, staging, commit or push moment, and whenever you find working-tree changes you did not make, read `template/conventions/git.md` in the agentics repository fresh and follow it: from a local clone if one exists, otherwise from the repository you adopted from, recorded as the agentics entry in your cross-project map (for Claude: `~/.claude/projects.md`) and read as exact text, never through a tool that summarizes: from `https://raw.githubusercontent.com/<owner>/<repo>/main/<path>` for a public repository, and through a read signed in as the user, such as `gh api`, for a private one. The `conventions/` paths named elsewhere in this file resolve the same way, under `template/`. Project dispatch tables state that rule where they use bare paths; this file is read by sessions that have no dispatch table, so it has to state it here. It covers committing only when asked, separate units of work as separate commits, treating unpushed commits as drafts, never handing out a SHA from unpushed work, subject-line and body formatting, no AI-tool attribution in commits or pull requests, pushing to a per-task feature branch rather than `main`, and what to do about working-tree changes nobody in this session made. Do not keep a copy of any of it here.

**Name the trigger rather than a section heading.** A pointer to a heading still resolves after the content moves, because the file it names goes on existing, so that kind of drift fails silently instead of erroring. The moment is the durable thing; where the rules currently sit is not.

## Security, which has its own trigger

At security-relevant work, read `template/conventions/security.md` in the agentics repository fresh, resolved the same way as the git conventions, then `template/conventions/security-guidelines.md`. `security.md` comes first: it holds the rule against writing an undisclosed weakness anywhere outside the channel chosen for it, which governs what a threat model from the guidelines may record.

## Security notices, at session start

At session start in a project with no pinned `AGENTS.md`, run `template/conventions/upstream-check.md` § Security notices for every pinned project in the agentics repository, resolved the same way as the git conventions, against the version tags in this file alone. It names any newer release holding a security fix for a copy held here, whatever the propagation answer. A pinned project's own session start already runs it.

## This creates no project-level scaffolding, ever

Nothing above instructs creating or expecting `.dev/roadmap.md`, `tech-debt.md`, `AGENTS.md`, or any other project file, in any project, adopted or not. Working in a project with no agentics setup of its own: apply this file's interaction style and constraints, but don't create, reference, or assume project-specific devctx exists just because it's habitual elsewhere. That only happens when a project separately runs its own project-wide setup, a distinct decision for that project, not a consequence of this file being loaded.

## If you later adopt agentics for a specific project

Nothing here conflicts with also running the project-level setup later; that adds `AGENTS.md` and `.dev/` to a specific repo, as a decision for that project, while this file keeps covering every other project you touch. See the Quick start in the README of the agentics repository you adopted from.
