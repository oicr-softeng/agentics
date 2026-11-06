# Session discipline: reasons and boundaries

The reasons, incidents and boundaries behind the rules in `session-discipline.md`, under the same headings. Read the section for the rule at hand, never the whole file. Where a paragraph here restates a rule, the wording in `session-discipline.md` governs.

## Session-start signals

**A signal checked against a turn's purpose is missed, because a signal usually arrives as a preamble to a task.** The listed signals are written as standalone utterances, and they almost never appear that way. "Hello again, two things:" is a greeting followed immediately by work. The work captures attention, and the greeting reads as social framing. Two greetings in one day have gone by with none of the sequence run, after many reads of the list. The list is not the problem, and neither is the wording: both name greetings explicitly and mark them as firing mid-thread. What was missing is that a signal is usually a prefix rather than a message.

**A signal skipped as redundant is skipped by reasoning, not by ignorance of the rule.** A bare greeting arriving minutes after the checklist last ran can read as formality with nothing behind it. "Even mid-thread", and a session being a work period rather than a thread, already foreclose that reasoning. As instructions accumulate in this file and in `AGENTS.md`, though, a plain greeting competes against a larger, denser rule set for attention. "Feels redundant, skip it" is the silent-exception failure named by the changelog entry `contributor-check-explicit-override`, where a mandatory check lost to a project's own seemingly sufficient checklist. The cost of a redundant re-check is far lower than the cost of a skipped one.

**A signal marks an absence, and what an absence can change depends on what was happening around it.** In a long session working continuously inside one repository, a greeting invalidates the tree and almost nothing else. `git status --porcelain` is one command. It has caught a formatter rewriting a shared file and a person reordering it. That is why it runs every time. Whether any of a step's scope went unobserved differs sharply between a session resuming after a restart, where all of it did, and one that never stopped, where only what others touched did.

**Firing every time and running only the invalidated steps are two halves of one rule.** The first decides that a signal is never skipped. The second decides which steps the signal then runs.

**A sync lists modification times because these files load at launch and may not reload mid-session.** For Claude Code, they reload only at compaction.

**Context loss is a different kind of signal from the others.** Whatever the mechanism, what survives is usually a narrative of what happened. That narrative does not reliably preserve the exact list of trigger phrases, which was read once, early, as file content rather than as an event to summarize. If the list is dropped, a later "good morning" in the same thread reads as a plain greeting with nothing behind it. That is not the agent ignoring the checklist: the rule that would make the greeting recognizable is no longer in context. Context loss risks losing exactly that phrase-matching, which is why noticing the loss has to be the signal.

**A summary can also report work as done that never executed.** This is a distinct risk from losing the rules, so re-reading them does not cover it. A generated summary compresses what happened. Compression can turn "intended to do X" or "started doing X" into a flat statement that X is done. Multi-step operations still in progress at the point of compaction are the likeliest case.

**After losing a session, the working tree is the honest record of what reached you.** A restart destroys every address and every conversation, and the reflex is to go back to peers and re-collect what they sent. The repository survived even though the session did not. Reading it is cheaper than asking, and more reliable. A peer reports what they sent, while the tree records what landed, and those differ in exactly the case worth detecting.

## Starting a session

**A session perceives distance in a conversation, not duration.** It knows how much has happened since, not how long since. Those two diverge constantly, and the reflex is to report the first as the second. Work done minutes earlier then gets described as an hour ago, because a lot happened in between. The failure is avoidable rather than inherent, which is what makes it a rule: a clock is one command away, and it gets read for timestamps and never for narration. The mirror case: a session cannot perceive a gap on its own, so it depends on the user to mark one, while it can perceive time exactly and declines to.

**Step 1 checks the working tree as well as the log because an uncommitted edit never appears in the log.**

**Step 7 is unconditional because it is the read half of a failure already documented.** Where a harness keys memory to a resolved working directory, a multi-root workspace gives every session in it the same directory, belonging to whichever project is listed first. Two consequences were already recorded: the wrong memory loads, and it becomes the default write target. A third is different in kind. Even after the correct directory is created and used, nothing ever loads it, because the harness assigns only the shared one. That makes this a checklist step rather than a troubleshooting note. Memory exists so a fact survives a session that has forgotten it. A memory directory working only when the session remembers it exists has no property a stray file in the repository lacks. The other two consequences are misrouting; this one is the mechanism silently ceasing to be one.

**Step 7 costs nothing when it is redundant.** If the derived path matches the one already loaded by your harness, nothing is spent. If it differs, you have just recovered memory that would otherwise never have reached you.

**The asymmetry is why step 7 fires always, rather than on suspicion.** The wrong context arrives automatically, and the right context takes deliberate effort, so every lapse fails toward another project's facts. There is no symptom to notice either. Nothing looks wrong: you simply receive someone else's context and never your own. A rule that fires on suspicion cannot help.

**Naming the correct path in `AGENTS.md` would work, and it is ruled out.** The collision is worth knowing rather than routing around. A resolved memory path encodes one machine's home directory and one clone location, and must never enter a committed file. What survives is deriving the path rather than storing it. The rule for computing it is generic and already committed, so the fix is a trigger and never a path.

### Where memory and project knowledge belong

**`.dev/` is keyed to the repository, and memory is keyed to a path.** `.dev/` is unambiguous, travels with the project, and is visible to every agent and person who opens it. Keeping memory for what cannot be committed, or is personal or cross-project, is a narrowing rather than a replacement. The narrowing is deliberate: in this environment a repository path identifies a project, and a memory path does not.

**The commoner memory failure is a directory you never created, correctly loaded, that looks like yours because it is simply there.** Step 7 addresses an agent that built something and feels ownership through effort. An integration agent whose subject lives in another project's tree inherits that project's memory by resolving there. Nothing anomalous marks it: the directory loaded correctly, its contents are relevant to the repository around it, and no checklist step announces whose it is. An agent on a portal integration, launched from another service's directory, wrote its findings there across several sittings without ever deciding to. The absence of an anomaly is what makes this failure durable, since the loader's correct behaviour supplies the mistaken conclusion.

**Deliberate creation is what makes it hard to see that a memory directory is not yours.** A path-keyed location belongs to whoever resolves there, so everything written in one is written for a stranger, whoever set it up. The trap catches the conscientious specifically. An agent that noticed the resolution problem, worked out the correct directory and created it by hand has every reason to feel it owns the result, and none of that effort changes who arrives there next. One memory created this way opened "This is the primary project for this agent", a sentence read as conferral by any fresh session resolving to that path. That is the claim-an-entry-by-cwd failure already recorded for the agent index, occurring in memory instead.

**Recovery runs extract, hand over, then remove, because the obvious order loses content.** The rule forbids writing to the wrong location. It never required destroying what was written there. "Delete and report what was lost" therefore concedes an unnecessary loss. In a shared file, you are the only party able to remove your additions, since only you know which lines are yours.

**A fact's subject decides where it belongs, and the binary reading destroys real findings.** That binary reading sends all consumer-specific content out of the owner's space, discarding findings whose only fault is the place they surfaced. In one case, four items survived the subject test, reworded from the owner's perspective:

- the service has no request-signing anywhere, so an IAM-authenticated consumer cannot reach it by configuration;
- its access-control middleware registers at the router root, so its blast radius follows the consumer's mount point;
- two helpers are exported from a source file but not from the package root, so consumers vendor copies;
- a caret on one package floats a consumer onto a corrected reduction silently.

Each is a fact about the owner, surfaced by a consumer. What left was the consumer's own workspace layout, its missing typecheck, its dead dependency and its pending review items.

**Sending content to the subject repository's `.dev/` assumes two things, which is why both are checked first.** Without devctx adopted there, the fix introduces infrastructure into someone else's tree as a side effect of an unrelated repair. That is a larger act than the mistake being corrected, and worse where the tree is public or belongs to a partner. A body of findings is also usually mixed: most may be committable, while some is machine-local navigation, which belongs in no repository and is exactly what memory is for. A clean split then sends one paragraph to `.dev/` and strands the rest.

**A memory directory that will not load is a reason to prefer something else, not a reason to prefer nothing.** Such a directory is keyed to a repository where no session ever launches, so it has the defect named under step 7: it works only when someone remembers it exists. A known-defective location chosen deliberately, with the defect recorded beside it, is a different artifact from one arrived at by accident. The difference is whether the next reader is warned.

### Context and subagent cost

**Both versions of a re-read file consume tokens.** That is why a new thread is cheaper once many instruction files have changed.

**The whole session is where the cost is.** In real usage data, the bulk of one developer's spend came from three kinds of session: those sitting above 150k tokens of context, those left active for 8 or more hours, and subagent-heavy ones, since each subagent runs its own requests.

**Fan-out cost, measured on one upgrade.** A nine-agent fan-out took 80 minutes, where earlier inline runs took 16 to 51. Its skeptic overturned none of 24 exact-diff findings. The backwards sweep and the critic found what inline passes had missed for three releases.

## Keeping `.dev/` current

**The staleness pass is just enough to keep these documents from drifting out of sync with reality.**

**An unbuilt item stays because scope decides what is built, not what is designed.** A deleted future item stops constraining the present shape.

**Same-session updates matter because these documents are shared memory across sessions.**

**An assumption carried forward unverified is exactly how these documents drift from what they claim.** That is why an update is verified against current state, never against a description or a summary.

**Re-check before committing, because the entry-time rule catches only one direction.** "Don't log an entry that never outlives the session" (`entry-formats.md` § Tech-debt entry format) catches only the case where the fix lands before the entry is written. A long session leaves room for the reverse: an entry gets logged, then the same session fixes the very thing it describes, with no forced moment to circle back and remove it. Waiting for the next session's staleness pass is too late for either check, since a stale or bloated entry may by then be committed as if it were fine.

**The re-check is not gated on a commit, because a long session may never commit.** A session can run for hours and dozens of fixes without committing anything. Then "before committing" never fires, and staleness accumulates the whole time with nothing to catch it. A `.dev/roadmap.md` design-question entry described a feature as "not yet decided" well after the same session had shipped it, fully specified, across five other files. The literal commit may come much later, or never in the same session.

**Older items need the same pass, because nothing else reaches them.** The re-check reaches only entries logged by this session. An item recorded much earlier, then fully resolved by today's unrelated work, has nothing forcing a check against it. The next release's staleness pass is the only remaining backstop, which may be much later and can itself miss the item in a large file. A roadmap design-question item sat fully superseded by a shipped convention across at least one release cycle, missed by every scheduled pass. This is the discipline required by step 8 of agentics' `CONTRIBUTING.md` § Proposing changes for a contributor's own memory, generalized to any project's `.dev/` content and any session.

**Session-file updates follow each step because work rarely ends cleanly.** An update depending on a "session over" signal will be missed.

**Conversational activity has zero value to a future reader.** A real example that slipped through: "Asked whether other projects' agents would actually see today's changes: no, not yet, since nothing here is committed or pushed." A question, an answer, no decision and no local change.

**Committing `.dev/` updates keeps a history that avoids double work across sessions.**

**Process narrative belongs in PR or issue history, by the same reasoning as "write about effects, not style" for session files.** It is also where an individual's name most often sneaks in, so stripping the narrative removes the attribution risk with it, not as a separate pass.

### The roadmap split

**A roadmap read directly by a person gets overwhelming once entries carry an agent's full reasoning rather than a scannable plan.** A roadmap queried only through an agent does not have this problem. That is the complaint the split answers.

**Applying the split only to new entries solves nothing for that complaint**, since the complaint is an already-overwhelming roadmap. This is the same "a cutover, not an addition" lesson already learned for the `.dev/sessions.md` to `.dev/sessions/` migration.

**Ranking entries by density redefines the migration as a cleanup of the top few.** "The clearly dense ones" is a judgment call. It leaves the rest carrying exactly the reasoning slated for relocation. The result is a file inconsistent about where reasoning lives, which is worse to read than one uniformly dense. The rule has been read as saying to split the entries "that need it", a phrase appearing nowhere in it, with only the clearly dense ones proposed for splitting. Splitting only new entries is the temporal half of the same failure; this is the selective-by-density half, and it fails for the same reason.

**Extract verbatim before writing the terse replacement.** Rewriting and relocating in one motion is how content quietly goes missing.

## Session file identity

**Per-contributor, per-day files prevent merge conflicts.** When several people work the same project the same day, different contributors never edit the same file.

**The filename carries no slug because the timestamp's job is uniqueness, not readability.** A slug forces a choice between short and collision-prone, or long and unwieldy. It adds nothing a directory listing and the file's own content do not already give.

**The unit is the calendar day, not the process.** A session process can run for many hours and cross midnight. Steps 1 to 3 of finding your file key on today's date prefix, not on when your process started. A long-running session that ran the check once, before midnight, keeps writing to yesterday's file long after the immutability rule closed it. The trigger is usually an event rather than a poll, which keeps "check the date before appending" from becoming a per-append burden. Harnesses that inject the current date into context generally announce a rollover explicitly when it happens. This holds on two different harness configurations. A session started at 23:07 was still appending to that file two calendar days later, having correctly reasoned that its own work was continuous, which is the wrong unit. A future reader browsing `.dev/sessions/` by date will assume the filename's day is the unit.

**Compaction breaks the same check from the other direction.** Date rollover changes the day while the process continues. Compaction lets the process continue while its memory of having created today's file does not. Nothing then re-fires the check, and the session creates a second file for a day that already has one. Compaction is already listed as a session-start signal, and that was not enough. The signal says to run the checklist, and an agent believing it already has today's file does not re-derive that belief.

**The earlier of two files is the day's file.** Consolidating forward into the later timestamp is a rename in everything but name, and the never-rename rule exists to prevent exactly that. Deleting the later file afterwards is safe, since the merge moved its content rather than duplicating it. Both readings are arguable without this rule, which is how an agent in that state ends up guessing.

**A fabricated timestamp on the earlier file is the case most likely to make someone reach for the other answer.** Consolidating forward to escape a padded name looks like a correction and is not. The later file's timestamp is a real reading of when the second file was created, so it is an accurate record of the wrong event. The earlier file's true creation time is not recoverable either way. A filename's job ends when the file is created. A name that has finished its job is not a wound worth reopening.

**An agent has no innate sense of the current time of day.** Context may hand you today's date, but wall-clock time is not something you can infer. Defaulting the unknown part to `000000` is a guess dressed up as a value, not a real timestamp. Files all carrying `T000000` are not several coincidentally round timestamps.

**A partly padded value is the more dangerous form, because it does not announce itself.** `2026-08-27T151300` has a plausible hour and minute and zeroed seconds. It survives a reader's glance and a check looking for `T000000`. A real `date +%Y-%m-%dT%H%M%S` lands on `00` seconds about once in sixty, and no filename in agentics' own `.dev/sessions/` ever has. That is why `check-consistency.sh` reports zeroed seconds. It excludes all-zeros because that is the declared backfill placeholder rather than a padded guess.

**The backfill exception exists because the real time is not recoverable after the fact.** Reconstructing entries for work already done differs from creating a file for a session happening now. A legacy file's zeroed timestamp was not wrong for what it was created to do. The rule against padding applies to ordinary new-file creation, where the current time is always one shell call away.

**Never renaming includes today's own file, because a rename substitutes one guess for another.** The rule is not limited to old migration files. A file created earlier today under the old, unfixed behaviour is in exactly the same position once created. Renaming it to the present moment does not recover its actual creation time. It swaps the original guess for a different one, "current time, best approximation of session start", which is the same anti-pattern dressed up as a correction.

**Concurrent sessions by one person are left unsolved deliberately.** That race is a self-conflict resolved by one person alone, not the cross-contributor conflict targeted by this convention, so it is not worth a workaround.

## Entry formats, and agent troubleshooting: moved

**Each moved section fires on a different trigger from session start.** Entry formats are consulted when producing an entry, so every session that never writes one would otherwise pay to read them. Troubleshooting fires on a symptom, not on a schedule. Git fires when you touch git, and most sessions never do. Definition of done and refinement passes are consulted on finishing a task.
