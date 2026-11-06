# Git

Read this when you are about to branch, stage, commit, or push, and when you find changes in the working tree you did not make.

Never commit without the user's explicit instruction.

Never stage changes (`git add`) without being explicitly asked to, either: staging is the user's call, same as committing. Making an edit does not stage it: changes sit in the working tree, unstaged, until staged deliberately. When reporting on changes made, state their actual git state plainly rather than just "the changes are there": a user who expects staging and finds none wastes real time looking for something that was never where they expected it.

**Say this in a reply, not into a persisted file.** A session log entry, roadmap item, or any other file that narrates its own current staging or commit state ("everything above is unstaged; nothing has been committed"), or its release or publish status ("not yet in a release", "shipped in vX"), goes stale once anything gets staged or released, and the file itself never gets corrected after the fact. `git status` (or the release process itself) already answers this live, for free, whenever someone reads the file later. Sharper still on a branch explicitly out of scope for versioning (a `main` that never carries a real version number): a file there has no standing to assert release status at all, since the branch never sees that fact. Describe work status (done, in progress, open), which the current branch does have authority over, and leave release status to whatever process actually promotes the work.

**Exception: a file that is the canonical record of release status, not a duplicate aside about it.** `CHANGELOG.md`'s own `## Unreleased changes` heading looks like the same violation at a glance, a release-status claim living on a branch out of scope for versioning, but it's the opposite case: nothing else tracks what hasn't shipped yet, so this heading isn't restating a fact `git status` or the release process already answers elsewhere, it is the one place where that fact is tracked, updated by that same release process the moment it changes.

**Unpushed commits are drafts, so collapse them the same way you collapse a note, per `documentation.md` § Rewrite freely until committed.** A series of commits arriving at a final state is the same failure one layer up, and each commit feels settled when written. Amend and squash freely until the work is pushed.

**Amending has a consequence outside this repo: it invalidates any SHA recorded from that commit.** Anyone who wrote that SHA down, in a file, a note or a message, holds a reference that stops resolving at the next amend. The two rules are compatible only in one direction: amend as much as you like, and never hand out a SHA from unpushed work, including in a message to a peer who may record it. **Read that as a hazard to yourself rather than a courtesy to them, because it is symmetric:** a SHA you gave away can come back to you as a citation you trust.

**What collapses is iteration on a unit of work, never the units themselves.** Separate units of work get separate commits, here and in every repo. This rule removes fixups, reversals, and "actually X" corrections, which only record how you arrived somewhere; it does not merge distinct units, which must stay apart for review, for bisecting a regression, and for reverting one thing without the others. The test is whether a commit would tell a future reader something absent from the final state: a correction to your own previous commit never does, and a genuinely separate change usually does. That test distinguishes iteration from a unit and does not size one. Passing it does not earn a change its own commit.

**A unit's size follows from why units stay apart, so a boundary needed by no review, bisect or revert does not separate units.** A vertical, meaning the changes across layers that together deliver one integration or capability, can be one unit whichever modules it touches. Split by layer, a vertical leaves each commit holding part of a capability, which is rarely worth reverting alone and may not build for a bisect. Two verticals stay separate units even where they share a module. Changes outside any vertical, such as standalone fixes, can share a unit within one module. Across modules they are separate units. A module here is a component with its own responsibility, such as a package or a service, rather than a single file or the whole repository. One change that spans modules, such as a rename or a fix to both sides of an interface, stays one unit. An incidental fix in another module can still ride along, as "Writing the message itself" allows. Tests and documentation belong to the unit they cover, wherever their files sit.

**A repo may publish at a coarser grain than a unit of work, and that is its own rule, not this one.** Somewhere that ships releases rather than commits can legitimately collapse everything between releases into one. That is a property of how that project publishes, so it belongs in its own contributor documentation, and it is not licence to squash unrelated work together anywhere else.

**A commit holds only what is meant to live on the remote.** Work needed by its owner alone, such as a placeholder standing in for a component not yet available, stays out through `.git/info/exclude`, per `testing.md` § Test support code: fixtures, peripherals and placeholders.

**A command run as a simulation can still move refs.** In git 2.55, `git replay` updates the branch it replays by default; simulate with `--ref-action=print`, and compare `HEAD` and the branch before and after any run meant to be read-only.

**Do not hard-wrap the body.** Write each paragraph as a single line and separate paragraphs with a blank line. **This deliberately departs from the widely documented git convention of wrapping at 72**, and the departure is recorded here so nobody restores it as a fix. That convention is a holdover from mail-based patch review, where the reader did no wrapping of its own. Today the forge web UI and every IDE client reflow the body to their own width, and `git log` prints it verbatim into a terminal that soft-wraps it, so a hard wrap at 72 supplies a second wrap that cannot agree with the first: breaks land mid-sentence and orphan words.

**The subject line is the opposite case and keeps a hard limit**, because nothing reflows it: roughly 50 characters, never past 72.

**Reflowing an existing message is a rewrite, and the SHA changes exactly as it would for a content change.** So it is safe only while the work is unpushed, on the same terms as amending freely, and the rule against handing anyone a SHA from unpushed work applies to a message-only rewrite without exception.

**Verify that a message-only rewrite was message-only.** The check is cheap and it is the difference between reflowing text and silently altering a tree: after rewriting, confirm the base commit is unchanged, that the `HEAD` tree and the working tree are what they were, and that rename detection still reports what it reported before.

**No AI-tool attribution in commits or PRs.** Do not add "Co-Authored-By," "Generated with," or similar trailers naming an AI tool or vendor to commit messages, PR descriptions, or PR comments, regardless of which agent is doing the work. Unprompted attribution reads as this project endorsing a specific commercial product; that's not something to do on any vendor's behalf, paid or not. **This overrides your own default commit-message template**, not just other projects' conventions: many agents, including Claude Code, are configured by default to append exactly this kind of trailer automatically. Verify the message against this rule as a discrete step when writing it, since a rule read earlier does not fire on its own.

**Writing the message itself.** Succinctness and the ban on procedural narration are in `writing-style.md` § Say what changed, not how you got there; where the same commit also adds a changelog entry or migration note, § One derivation, one owner decides which of the two carries the reasoning and which compresses to a pointer; what is git-specific follows. One line for the subject, a body of two or three sentences for an ordinary change, longer only for a real, specific reason (the bundled-fix case below, or agentics' own release-commit format in `CONTRIBUTING.md`). Check this repo's own recent commit history for its actual *format* conventions, a type prefix like `feat:`/`fix:`, capitalization, tense, and follow that. A repo's history proves a format convention exists, it doesn't prove the length is any good, only that no one has fixed it yet, so don't carry an already-verbose habit forward as if it were a discovered rule. Subject line describes the user- or operator-visible effect, not the internal mechanics: "add per-catalogue nesting config for enveloped data sources," not "add nestingPrefix.ts and thread it through ten files." Body is prose, not a bullet list restating the diff; if an incidental, unrelated fix rode along with the main change, name it in one clause, not a paragraph, honest about scope without losing brevity. Stage only what's logically part of the unit of work before writing the message: an overly broad diff is often exactly where extra message length actually comes from.

**Two approved examples, and the actual trigger that produces a bloated one instead of matching either.**

```
test: adopt supertest and pilot fast-check

Replace manual HTTP server boilerplate in six route test files with
supertest. Add fast-check property tests for the GraphQL name
sanitizer as a scoped pilot.
```

```
fix: resolve documentType to catalogueId in GraphQL routing

Multicatalogue GraphQL requests now resolve an ambiguous or unique
documentType the same way introspection already does, instead of
404ing on a path that names only a documentType.
```

What the second would have looked like carrying a session-file entry's register over:

> Investigated why multicatalogue GraphQL routing was 404ing on requests naming only a documentType. Traced it to `arrangerRoutes.ts` missing the same `findCatalogueByIdentifier` wiring `introspection/index.ts` already had from an earlier commit, which added tests for both call sites but only wired one.
>
> fix: resolve documentType to catalogueId in GraphQL routing

The trigger is not the difficulty of the work but carrying the session file's fuller register (the reasoning, what was tried, how it was confirmed) into a message that should describe only the resulting effect. Check which register you're about to write in as a discrete step before finalizing the message, not just which facts to include.

**For a security fix, the commit message and the PR description follow `security.md` § Releasing the fix for a hole carried by a published release until the release ships: what the code must do, never how earlier versions fail.** A PR description is one of the change records exempted in `documentation.md` § Rewrite freely until committed, so it invites the "before" state, and for a security fix the "before" state describes the hole.

**Pushing defaults to a feature branch, not `main`.** When asked to push (in any session, local or remote), push to a per-task feature branch named descriptively after the work done (`feat/env-var-wiring`, `fix/merge-config-spread`), never to `main`, `master`, or a release branch, unless explicitly instructed otherwise. **Branch before committing if you are on the default branch**, not only before pushing: committing on `main` and then pushing a feature branch leaves local `main` ahead of its upstream, one plain `git push` away from publishing exactly the work meant to stay off it. The exception is a repository's first commit, which goes on its default branch, since branching first would leave that branch unborn.

A project with a different commit, staging or branching discipline states it in its `AGENTS.md` § This project's constraints, and the user can state their own in their global context.

## Unattributed working-tree changes

A shared local clone can pick up uncommitted changes made outside this session: a different, concurrent session working the same repo, another tool, a teammate's local edit. Their presence in the working tree isn't evidence they're correct, safe, or already reviewed, only that a `git status`/`git diff` will show them.

Notice this at session start (see `session-discipline.md` § Starting a session) and any other time it comes up, most commonly the user pointing out that a file changed unexpectedly. Either way, treat unattributed content as you would any unverified claim (see `review-conduct.md` § "Ground truth over claims"): validate it before trusting it, or before telling the user it's fine.

**What "validate" means here, concretely:** read the actual diff, not just its own description of itself. Check it against the same bars as your own work: technical correctness, the project's Critical Constraints (no credentials, no personal information), format and cross-reference consistency with existing conventions, and whether it duplicates or conflicts with something already present. Report the specific checks made and what each found, not just a verdict.

**A dirty working tree says an edit happened, not that anyone is working now.** `git status` carries no timestamp and no author: an uncommitted change may be minutes old and actively in progress, or left behind weeks ago by a session that has long since ended. Do not infer a live concurrent session from it, in either direction. Reading it as "someone is mid-edit" can stall work nobody is actually doing, or route a request to a session that no longer exists; reading it as "stale, nobody is here" can clobber work in flight. If whether someone is active actually changes what you do, ask, or check for a live session; do not derive it from the working tree.

**A validation step, not an automatic revert.** Content that passes validation can be kept. Whether to keep it, revert it, or hold it for their own review is the user's call, informed by what you found, not decided by the fact that it arrived through an unreviewed channel.

**Not limited to git's own working tree either.** The same kind of surprise happens in any persisted location shared by more than one session: a memory entry, a `.dev/docs/atlas/` write-up, a draft, that this session didn't write and doesn't recognize. "Someone changed this, wasn't us, will ignore it" is not the same response as validating it, it's a third, worse option, neither trusting it nor checking it, just dismissing it. Apply the same discipline regardless of which layer it showed up in: read it, validate it against the same bars above, then decide whether to keep, revise, or flag it for the user.

**Two failure directions, and the second one accuses somebody.** Dismissing an unexplained change is the error named above. The opposite is treating a legitimate edit as an attack: content you wrote is later corrected by another authorized party you have no visibility into, and the correction reads as tampering or a hostile reversal. An edit to a shared memory file, flagged to the user as tampering, came from the agent owning that memory space, correcting the record about its own role, on the user's own instruction.

From inside a single session, "this content is false" and "this is a legitimate edit by an authorized party I cannot see" look identical, and no amount of care distinguishes them, because the missing information is authorization, which is not in the file. So the discipline is not to judge harder, it is to change what you say: describe what changed and that you did not write it, ask who did, and let the user supply the authorization you cannot observe. Reserve words like tampering, reversal, or hostile for cases where you have actual evidence of intent rather than an absence of explanation, since raising an alarm about a colleague's authorized work has a social cost that a neutral question does not, and it lands on the user either way.

