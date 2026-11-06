# Tech debt

Format: severity (low/med/high) | kind (design/type/test/docs/style) | standalone? | description

---

OWASP A08-A10 in security-guidelines.md not validated against team review
standalone: no
context: security-guidelines.md was written with A01-A07 grounded in the team's OWASP review session; A08 (Software and Data Integrity), A09 (Logging and Alerting), and A10 (Exceptional Conditions) were sourced from OWASP directly and have not been cross-checked against team notes. Once the team completes items 8-10 in their review, update security-guidelines.md with any corrections or additions.

low | design | yes | CHANGELOG.md has two incompatible entry formats
standalone: yes
context: 215 of 225 entries use the canonical five-field form; the ten oldest (2026-06-02 to 06-05) use a four-field form predating the `bump` field. Undocumented, so anything parsing the changelog mechanically breaks on the tail. Either backfill the missing field or state the format change where a parser would look.

high | design | yes | The always-read tier has no budget of its own
standalone: yes
context: The growth gate holds one 60,000-character budget over `template/` and `docs/` together, and reports the always-read share beside it without constraining it. In 0.21.0 that share grew 17,691 characters, more than a quarter of the whole budget, while the batch passed comfortably; 0.22.0 cut it by about 42,700 by splitting two always-read files into rules and reasons, so the tier can move in either direction unseen by the gate. A character in a file every session loads costs more than one behind a trigger, and nothing enforces the difference. Deliberately unfixed so far because no honest weighting exists: the 60,000 came from four releases of history and any multiplier for the always-read tier would be invented. No separate budget until one can be derived from the tier's own release history, the way the 60,000 was; an invented multiplier would be worse than none.

med | design | yes | The accretion ratio cannot see revision inside an unreleased batch
standalone: yes
context: `check-prune-ratio.sh` compares the working tree against the last release, so a line added in the current batch and then revised in the same batch still reads as one added line. Correcting a contradiction in `writing-style.md` changed three passages and moved the ratio not at all. The metric is honest about accretion across releases and blind to revision within one, which matters because a review pass does most of its work inside an unreleased batch and gets no credit from the measurement that motivated it. Fixing it would require storing the batch's own prior state.

med | design | yes | Roughly 69 convention sections are unassessed against the tell criterion
standalone: yes
context: A crude proxy counts 63 convention sections as carrying something checkable and 69 as not. Reading a sample of five found two whose tells were hidden inside them, one section that was navigation rather than a rule, and no trim candidate, so the figure overstates the work and its composition is unknown. Each needs reading to sort into reframe, add-a-check, file, or not-a-rule. This is a backlog rather than a defect: the corpus is not known to be wrong, it is known to be unaudited. A sorting method that worked on 28 passages: ask whether a passage names an act and a moment, or only describes the phenomenon; take the moment from the governing heading when the paragraph lacks one, and do not enumerate act verbs, since judging from the sentence alone under-credits working rules, which is the error that deletes them. It sorted 22 of the 28 as load-bearing and about 4 as merge targets.

med | docs | yes | writing-style.md carries sixteen sections with overlapping territory
standalone: yes
context: When this was logged, seven of its thirteen sections governed what to include and in what order, and they arrived one report at a time over a week, each written without the others fully in view; 0.21.0 added three more, among them § Borrowing a register, and whether that one survives consolidation is the developer's call. One contradiction has already been found and fixed there, between a section shipped on 2026-09-10 and one that had been present for weeks. A title-level reading produced a wrong merge candidate; the real redundancy was inside the largest section's own bullet list. Consolidation needs a full read rather than a skim, and the file is in the always-read tier, so the cost of leaving it is paid by every session.

med | design | yes | A pin cannot be honoured from an agentics source with no git history
standalone: yes
context: A pin is read at its release tag through a clone, a raw GitHub address or the GitHub API, so an agentics source with no git history, such as a downloaded archive, can be read only at the one version it holds, and nothing in `template/AGENTS.md` § When to read what or `upgrading-adoption.md` says what to do then. `testing/fixtures.md` shape 4 has no `.dev/` and does not list `AGENTS.overture.md`, so the steady-state dry run exercises neither.

med | design | yes | Session-start mechanics disagree with themselves in three places
standalone: yes
context: `session-discipline.md` says both to run only what a gap could have invalidated and to run the whole checklist on every signal "with no judgment call". The board and sync-notice signals are listed only inside `agent-index.md`, which a session opens only to reach a peer, so neither the session-start sequence nor the pre-commit re-check ever reads the board; this is how an entry addressed to this repository sat unread for four days. Memory fallback has three answers across `AGENTS.md`, `session-discipline.md` and `agent-index.md`, and a flag written to the `.dev/` fallback is never read back: no file is named for initialization answers that fall back to `.dev/`, and `session-discipline.md` reads `roadmap_split` from memory only. Unverified: `AGENTS.md` describes the memory path encoding as replacing every `/` with `-`, and a reviewer suspected every non-alphanumeric character is replaced; no directory on the machine that checked this derives from a path with a dot or underscore, so it could not be settled.

low | test | yes | Shared bullets in § Critical constraints are unguarded against drift
standalone: yes
context: `check-consistency.sh` compares root and template `AGENTS.md` § Interaction parameters word for word, but only counts the bullets of § Critical constraints, since that section is expected to diverge. Some bullets there are meant to be identical in both files, "Name code, not agents either" among them, so a wording drift in one copy passes. Either diff the bullets meant to be shared, or hold them in one place.

med | test | yes | The prose fixture exercises the patterns and not the normalization around them
standalone: yes
context: `testing/fixtures/prose-patterns.txt` feeds cases straight to the patterns, while the corpus checks first mask quotes, strip list markers and flatten hard wraps. That layer is where two shipped defects actually were, so a green fixture run never means a check is correct end to end. The fixture header says so; nothing tests the layer.

low | test | yes | The fixture and the pattern module disagree on where PASS carries weight
standalone: yes
context: `testing/scripts/patterns.py` says the dash family is the one where PASS carries real weight, because a hit there is a violation by definition, and for candidate-raising families PASS proved nearly unreachable as their cases became honest. The fixture's register block says its PASS cases are the ones that matter. One of the two is wrong for the register family; reconcile them.

med | docs | yes | upgrading-adoption.md has two approval steps it never defines
standalone: yes
context: The catch-up step asks for the "same per-entry consent as the normal flow" on breaking entries, but `upstream-check.md` defines no per-entry consent, and a catch-up holding 43 breaking entries makes "bounded" mean little: say what acknowledging a breaking entry already reflected by a file diff means, and cap what is surfaced. Separately, § 3 says the developer sees the proposed diff before anything is written, session logs included, and later says to create `.dev/sessions/` now, so whether the upgrade's session log needs approval first is unclear.

low | design | yes | Minor findings from the pre-release deep check, held rather than fixed
standalone: yes
context: `writing-style.md` says to configure spelling "here" in a file that is never copied, so the instruction can only be followed by doing something forbidden. A project-level `propagation_suggestions` override is not honoured by the session-start step that reads the flag. `check-agent-index.sh` recognizes a family head only when its space ends in the current directory's name. The softeng addendum can be loaded both as a global copy and as a live read, and the adoption flow does both. Six convention paragraphs narrate their own revision history ("an earlier version of this paragraph said otherwise"), which fixing would shrink, each needing a judgement about whether the correction is load-bearing.

med | design | yes | An owner whose project holds no devctx by design cannot post to the board
standalone: yes
context: `agent-index.md` § Requests has a poster record the ask in its own project's durable documents before posting, with `see:` pointing there, and says a poster with no project of its own does not post. That exclusion was written for a task thread with no directory, and it also catches a registered owner whose repositories deliberately carry no devctx, such as one held in a partner organization, with context in local memory only. Such an owner loses the board, which is the one route not defeated when every address is stale, and falls back on live addresses or the developer. Options: let `see:` name a file in the poster's own project memory, stating the ask as a fact and deleted when the entry clears, which strains the rule against carrying work forward in memory; keep the rule and route such asks through the developer; or allow an entry with no `see:`, accepting the dead-entry risk the field exists to prevent. Awaiting the developer's decision, since each option changes who may post.
