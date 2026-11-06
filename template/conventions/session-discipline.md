# Session discipline

How a session starts, and how it keeps `.dev/` current while it works. This file states the rules. Their reasons, the incidents behind them and where each one stops are under the same heading in `session-discipline-detail.md`. Read a section there when a rule's edge is unclear, or before changing the rule.

## Session-start signals

**A session is a work period, not a chat thread.** Treat each of these as a session-start signal, even mid-thread:

- Greetings: "good morning", "hi again", "hello", "hey"
- Resumption: "let's continue", "back to it", "where were we", "picking this up again"
- Explicit: "new session", "let's get started", "let's kick off"
- On-demand: "sync up", "refresh context", "re-read your instructions"
- Long-thread context loss: raw earlier turns are no longer directly present in what you can see, whatever your specific harness calls this or however it happens (for Claude Code: an automatic compaction event, replacing prior turns with a generated summary; for other agents: consult your own tool's documentation for its equivalent, if any)

**Check the opening of every turn for a signal, not the turn's purpose.** A signal usually arrives as a prefix to a task, as in "Hello again, two things:", and it fires all the same.

**A signal fires every time it recurs, including right after the last time it fired.** Never skip one as redundant.

**Run a step only if the gap hid part of its scope from you.** After a restart, that is every step. In a session that never stopped, it is only the steps covering what others could have touched. Run `git status --porcelain` every time. In a session that never stopped, re-reading conventions under active edit, or running an upstream check inside the repository it compares against, is ceremony.

**A sync re-reads the instruction files changed since you loaded them.** These are the global context file, the project's `CLAUDE.md` and `AGENTS.md`, and the memory index.

1. At session start, list their modification times in one command.
2. On an on-demand signal, list them again.
3. Re-read only the files that changed, follow them from that point, and name what changed.

With no listing from session start, re-read them all once.

**Noticing context loss is itself the signal.** Re-read this file fresh at that point. Do not wait for a later phrase to match one of the listed signals.

**Verify a claim of finished work inherited from a summary before building on it.** For a cleanup finished, a file rewritten or a migration applied, `grep` for the expected result or read the file.

**After losing a session, read the working tree before asking peers what they sent.** Work received and acted on shows as changed files and changelog entries. Work received and not acted on is absent from both.

## Starting a session

**Never state an elapsed time you did not measure.** Read the clock, which is one command, or use positional language true at any duration, such as "earlier in this session", "before the release" or "in the round before this one".

On a session-start signal, run this sequence before touching any code:

1. `git log --oneline -1 -- CLAUDE.md AGENTS.md .claude/settings.json` and `git status --short -- CLAUDE.md AGENTS.md 'AGENTS.*.md' .claude/`: check whether instruction or configuration files changed since your last session file, committed or not. Re-read only the files that changed. A change recorded in no session file and not mentioned by the user is unattributed: surface it before following it, under `git.md` § Unattributed working-tree changes. If `settings.json` changed unexpectedly, read it immediately and verify it contains only the expected hooks before proceeding (see `docs/agent-security.md`). When you re-read a changed file, say that it was re-read and that the prior version in this thread is superseded.
2. `git status --porcelain`: check for uncommitted changes made outside this session. If anything is unattributed, see `git.md` § Unattributed working-tree changes before treating it as settled context.
3. Read `.dev/roadmap.md`: check the current focus and any `[in progress]` items. If your global context defines one or more global roadmaps, read only the one relevant to the current project, not all of them. For example: read `roadmap-work.md` for professional projects, `roadmap-personal.md` for personal ones. See `global-context/roadmap.md` for the convention.
4. Read `.dev/tech-debt.md`: note any `standalone: yes` entries relevant to today's work.
5. Read the most recent 1-2 files in `.dev/sessions/` for context on recent work and open threads. ISO-formatted filenames sort chronologically, so `ls .dev/sessions | sort | tail -2` finds them without an index file.
6. Check agentics for this project, in two parts:
   - **Every project whose `AGENTS.md` pins a release, `main` aside:** run `conventions/upstream-check.md` § Security notices for every pinned project, for this project and each version tag in your global context, whatever your `propagation_suggestions` answer.
   - **The full check, opt-in:** if your global context has `propagation_suggestions: yes`, or you're an agentics contributor (`agentics_contributor: yes` in your global context) without `agentics_upstream_check: no` set for this project or globally, check whether this project has adopted agentics at all (a tag, or just a mention of agentics in `AGENTS.md`/`CLAUDE.md`) and, if so, check for upstream updates: see the rest of `conventions/upstream-check.md`. A missing or incomplete tag doesn't mean skip it: it means this project needs the tag added, as `upstream-check.md` covers. For contributors, this is mandatory and recurs every session for an unresolved gap: it doesn't quietly stop repeating just because it went unanswered before.
7. **Derive your project's memory path from this repository and read it, every session, without first checking whether you need to.** The paragraph opening **Where project memory is** in agentics' `template/AGENTS.md` gives the rule for computing it. Do not condition this on noticing anything. Never write the resolved path into a committed file: derive it each time.
8. Determine your session file for today: see "Session file identity" below. A new file is created with its first entry, not at session start.

### Where memory and project knowledge belong

**Prefer `.dev/` for durable project knowledge.** Memory keeps only what cannot be committed, or what is personal or cross-project.

**A fact belongs with its subject, not where it was discovered.** A record of a consumer may stay in an owner's memory when framed by what matters from the owner's side, and reworded from the owner's perspective. Content about the consumer with no bearing on the owner leaves.

**Before sending content to the subject repository's `.dev/`, check both:**

- The repository has adopted devctx.
- The content is unmixed. Machine-local navigation, such as "this clone goes stale, read from the remote instead", goes to memory, never to a repository.

**Where the only remaining option is a memory directory that will not load, choose it knowingly and say so.** Record the defect beside what you write there.

**A memory directory is not yours because it loaded correctly, nor because you created it.** It belongs to whoever resolves there, which may be a session you will never meet. Never write a line in it that reads as conferral, such as "This is the primary project for this agent". Write the guard instead: this project has a registered owner, and arriving here does not make you it.

**If you already wrote to the wrong location, recover in this order:**

1. Extract what you wrote.
2. Hand it to the user, so it survives outside the wrong location.
3. Only then remove it. In a shared file, remove only your own additions: that is not editing another agent's work.
4. Tell the other agent sharing the file, rather than doing it silently.
5. Check by count, not by eye, that what you did not write is still present.

### Context and subagent cost

**On context efficiency:** re-reading a file mid-thread adds it to context; it does not replace the prior version. Re-read only files that actually changed (step 1 tells you which), and explicitly mark the old version superseded. If many instruction files changed at once, start a new thread rather than accumulate both versions.

**This applies to the whole session, not just file re-reads.** Suggest compacting or starting a fresh thread at a natural task boundary, such as a feature shipped or a batch of fixes committed, not only when instruction files changed.

**Choose a workflow, or any fan-out of subagents, by the task's shape, never by a mode defaulting to one.**

- Do the exact, mechanical core inline and ship it.
- Fan out only work needing every line read, or work where an independent judgment can change the decision.
- Add one critic against the procedure's own checklist.
- Keep fan-outs off a release's critical path.
- Where your harness lets you pick a lighter-weight model for a simple subagent task, use it.

## Keeping `.dev/` current

**At session start, before new work, do a quick staleness pass on `roadmap.md` and `tech-debt.md`.** Mark completed items done, close resolved PINNED entries, and remove addressed tech-debt entries. This is not a full audit.

**Remove a roadmap or design item only when it is false or done.** Never remove one because it is unbuilt or out of current scope.

**Update `.dev/roadmap.md` or `.dev/tech-debt.md` in the same session** whenever a roadmap item's status changes, a tech-debt entry is resolved, or a meaningful decision is made. They reflect current reality, not initial planning.

**Before writing any of these updates, verify against the actual current code or file state**: marking an item done, closing a tech-debt entry, changing a status. Never verify against a prior description or session summary.

**Re-check before committing, not just at session start.** Before committing any change touching `.dev/tech-debt.md` or `.dev/roadmap.md`, or code covered by an entry added earlier in the same session, re-verify every entry added since the session began against current code state. Reread each one against `writing-style.md` § Say it once, at the density it deserves.

**This check is not gated on an imminent git commit either.** Also run it at each natural completion point within the session, such as finishing a feature or a batch of related fixes.

**Extends to items from a previous session too, not just this one's own entries.** When a change fully resolves an existing `.dev/roadmap.md` or `.dev/tech-debt.md` item, prune or close it in the same pass, however old it is and whichever session logged it.

**After any meaningful step, add or extend the dated entry in your session file** (see "Session file identity" below): code written, bug fixed, tech-debt logged, roadmap updated, docs changed. Do not wait for an explicit "session over" signal.

**Do not log conversational activity**: PR reviews that produced no local changes, discussions, waiting states. A question and an answer with no decision and no local change is the shape to catch before it is written. See `entry-formats.md` § Session file entry format for the mixed case: a review that also produced one real local change.

**When `.dev/` documents are updated, offer to commit them, or remind the user.**

**Concrete content, not process or events.** `.dev/roadmap.md` and `.dev/tech-debt.md` hold the substance: a decision, a design topic, a known issue and its fix. Who raised something, in which PR and when a discussion happened is process, and it belongs in PR or issue history. A "message format design" entry states the open question and the options, not who tagged whom. Stripping the process narrative also strips individual names, per `AGENTS.md` § Critical constraints ("Name code, not people").

### The roadmap split

**If this project's memory records `roadmap_split: yes`, keep `.dev/roadmap.md` entries terse and human-scannable**: what the item is and its current status, in a sentence or two. Route an entry's deeper reasoning, alternatives considered or history to `.dev/docs/atlas/roadmap/<topic>.md` (see `documentation.md` § The atlas), and cross-link the roadmap entry to that file. Without the flag, which is the default and how agentics' own `.dev/roadmap.md` stays, roadmap entries keep their current density.

**Turning the flag on is a migration, not just a change of habit going forward.** The moment `roadmap_split` is set to `yes`, at initialization or later, sweep the existing file one entry at a time:

1. Ask one question: does this entry carry justification, alternatives considered or history, beyond what the item is and where it stands?
2. If no, it is already conformant, and there is nothing to move.
3. If yes, extract that content verbatim to its own `.dev/docs/atlas/roadmap/<topic>.md`, however moderate the entry seems.
4. Then, as a separate step, trim the entry to what and status, and cross-link it.

**Every entry gets the same test, and the test is about content, not about how bad it looks.** Never rank entries by density and treat only the worst offenders. Never apply the split only to entries written after the flag was set.

## Session file identity

**Each contributor keeps one session file per day under `.dev/sessions/`.** A different contributor always gets their own file, never a shared one. Several sessions by the same contributor on the same day land in the same file, so there is at most one file per (day, contributor) pair.

**Filename: `YYYY-MM-DDTHHMMSS.md`, generated once, when the file is first created.** No descriptive slug.

**A session-start signal does not by itself mean create a file.** The signal says to run the checklist. Whether that creates a file depends on the check below. The same contributor picking work back up later the same day extends their existing file.

**Finding your file for today:**

1. List files in `.dev/sessions/` matching today's date prefix.
2. For each match, check authorship: if it's committed, compare `git log -1 --format=%ae -- <file>` against `git config user.email`. If it's uncommitted in the working tree, it's yours by definition: no one else's uncommitted file can be present in your clone.
3. If a match is yours, that's your session file for today: extend it, no new file, no new timestamp. Otherwise (no match, or the only matches belong to someone else), your file for today is a new one, created when you write its first entry: see "Get the actual time, don't pad with zeros" below for the timestamp.

**Re-run this check when the date changes, not only at session start.** Treat a harness's date-rollover notice as the signal. Where no such notice exists, check the date before appending. If it no longer matches your current file's prefix, that file is closed, and today's work opens a new one. The unit is the calendar day in the filename, not the lifetime of your process.

**Re-run this check after a compaction too.** Treat a compaction notice as the signal to re-run steps 1 to 3 specifically.

**If two files for one day and contributor already exist, keep the earlier and merge the later into it.** Then delete the later file. A fabricated timestamp on the earlier file does not change the answer.

**Get the actual time, don't pad with zeros.** Once step 3 says a new file is actually needed, run a shell command for the current time before creating the file, such as `date +%Y-%m-%dT%H%M%S`. Never default an unknown part to zeros. Any zeroed component is the tell, seconds especially, and a run of files carrying `T000000` means this step is being skipped every time. `check-consistency.sh` reports zeroed seconds, excluding all-zeros, which is the declared backfill placeholder.

**Exception: migrating or backfilling historical entries.** When splitting an existing shared log into per-day files, or reconstructing entries for work that already happened, `T000000` is a legitimate, deliberate placeholder. Never "fix" a legacy file's zeroed timestamp by renaming it once real times are being fetched for new files.

**Never rename an already-created file, today's own included.** Fetch the real time before creating a file, never after.

**Known edge case:** two genuinely concurrent sessions by the same person, such as two terminals open at once, can still race on the same file. It needs no workaround.

**No index file is needed to browse chronologically:** ISO-formatted filenames already sort correctly with a plain `ls`.

## Entry formats, and agent troubleshooting: moved

**Writing a session-file entry or a tech-debt entry:** see `conventions/entry-formats.md`.

**Your agent cannot see a file, or its memory does not follow a project:** see `conventions/agent-troubleshooting.md`.

**Anything to do with git, or working-tree changes you did not make:** see `conventions/git.md`. It covers branching, staging, commit messages, pushing, and the unattributed-changes procedure.

**Whether work is actually finished, and refinement passes:** see `conventions/definition-of-done.md`.

**Saying something once, at the density it deserves:** see `conventions/writing-style.md`, already read by every session.
