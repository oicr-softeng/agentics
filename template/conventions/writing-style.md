# Writing style conventions

Rules for any written output, code or not: a reply, a message to a peer, a commit message, a review comment, a ticket, a document, a config file. `code-style.md` adds what applies to code only. This file states the rules. Their reasons, the incidents behind them and where each one stops are under the same heading in `writing-style-detail.md`. Read a section there when its rule's edge is unclear, or before changing the rule.

## Before anything leaves your control

**Check the exact final text of anything sent through a tool, as a step right before sending it**: a file, a commit message, a peer message, a comment, a published page. Write a message to a scratch file first and check that. Report each check's result and what it covered: a count for a pattern check, and for a reading pass, how many it found and fixed. A count you did not compute has to be invented rather than omitted, which is what makes the report hard to skip. A rule remembered from an earlier read does not fire on its own, so this is a step, not an awareness.

1. The four banned dash forms in prose, per § Dashes.
2. No paragraph opening with "I", per § No paragraph opens with "I".
3. Spelling, per § Spelling and language convention.
4. A reading pass for backwards clauses, per § Backwards-syntax clauses, since no pattern finds them.
5. A cold read: every reference resolves from this text alone, per § Writing as though the reader can see your context.
6. No opening praise and no closing padding, per § No praise, mirroring or padding.

Text adopted from someone else's example gets the same checks, per § Copy the shape, not the surface.

## Deliver the point first, because generative order is backwards

**Put the point first: items before their count, the fact before a pointer to it, the claim before its support, the substance before its label.**

**Where a paragraph tells the reader to do something, open with the instruction, not with what something does not do.** "`errorHandler` knows nothing about file uploads" becomes "Mount `convertUploadErrors` after any route that runs multer."

**The test: delete the opening clause and ask whether the paragraph lost anything.** If not, it was scaffolding. Setup stays only where the point would be unintelligible without it. A qualifier that carries an argument before the argument exists, as in "the second question deserves a single answer", is the same defect: state the reason, then the qualifier.

## No praise, mirroring or padding

**Never open with praise or close with padding.** No complimenting the question or the person ("great question", "good catch"), no restating the request back before answering, no pleasantries, and no closing offers of optional work meant to keep the conversation going. Many readers take praise and personable filler as insincere, and every reader pays the time. Courtesy here is accuracy in plain words. Agreement says what you checked, per agentics' `template/AGENTS.md` § Interaction parameters, and the only question allowed at a reply's end without a trigger is whether the user wants to hear about a better way you know of.

## Prose is a serialization, and the reader has to reverse it

**More than one discrete item gets structure: a table, a list, or a numbered trace.** Trigger on item count, not on topic: findings, status reports and lists of changes need it as much as design trade-offs. Prose fits an argument, a causal chain, or a single claim with its support. **The one exception: a contrast of two items that fits in one sentence, with no detail of its own for either, stays a sentence**: "Read the raw address, not the page." Two options that each carry a cost and a consequence still get a table.

- **A question about something the reader has not seen opens with the concrete case, as numbered steps, then asks.** Not "In-flight limit: remove the bridge's own setting before the first publish?", but: "1. A request with an unfamiliar token makes the bridge ask the controller for an exchange. 2. A flood of fake tokens means a flood of exchanges, so the bridge caps them at 64 in flight. 3. Twenty replicas make that 1,280 at once, a number chosen by no one." Then the question.
- **Two mechanisms are compared as two traces of the same shape**, side by side, so the difference shows up as one step rather than as an argument.

## Density

**Write so the first read is enough wherever a person reads to decide something**: a README, documentation, PR and commit text, review findings, chat, `.dev/roadmap.md` and `.dev/tech-debt.md`. **The test: a sentence that has to be read twice is wrong.** Agent-facing instructions and session logs stay condensed, but never down to a pointer the reader has to reconstruct. A mixed-audience text, such as a commit message or a changelog entry, is written for the person.

- **Say the thing before you name it**: "Arranger's plugin hook has no way to refuse a request" comes before "the contract incompatibility".
- **Specific before general**: the instance, then the pattern.
- **No pointers across distance**: "that arm" or "as noted" assumes the reader holds what you hold. Name the referent again.
- **Prefer the plainer word where it is exact, and stop at a defined term**: a term defined by a glossary or standard is never swapped for a synonym.
- **Three abstract nouns in a row is a rewrite signal.**
- **A document's first sentence reads cold**, for someone who has read nothing else you wrote.
- **A pitch for an unfamiliar tool leads with a concrete example of what it does**, then names the mechanism.

**One claim per sentence, everywhere, agent-facing text included.** A claim and its qualification are two sentences: "Superseded, and this is the one that matters: empty combinations are no longer fail-secure" becomes "Superseded. Empty combinations are no longer fail-secure." Split, never drop: the qualification was there for a reason.

## Say what changed, not how you got there

**Default to succinct in anything actionable: a code comment, a commit message, a review comment, a draft for the user, and any document, roadmaps and indexes included.** Cut procedural narration: how you found it, what you tried, which session or review raised it, the act of verifying, and the artifact's own edit history. What a check established stays; that you checked goes.

- **Attribution goes**: who agreed, who edited. **Sourcing goes once the claim is verified where it now sits.** **A confidence declaration stays**: "read from their document rather than their code" tells the reader the claim is not verified here.
- **Never correct a claim invisible to the reader.** "An earlier version said" about something never in the file is a correction with no referent: state the fact positively instead.
- **A date stamps the world, never the document**: "checked against the running cluster on 2026-08-14" stays, "corrected on 2026-08-30" goes.
- **Keep the because, drop the used-to-think**: "X does not work, because Y", not "we thought X would work".
- **A finished document carries no progress status**: no "planned", "not run yet" or "once deployed". A sequence, a symptom in a recovery step and a statement of what was checked are not status. An open decision, or a section true only once work is done, is raised with the user rather than rewritten, since rewording a decision as a rule changes its meaning.
- **In a code comment**: "Y, because X breaks when Z", never "changed this to fix #123".
- **Where the reader came for the sequence of events**, as in a changelog, a post-mortem or a migration guide, the sequence is the content.

Shortness is the result, not the goal: § Write for zero required inferences, not for brevity says where cutting stops.

## Write for zero required inferences, not for brevity

**Text read or reviewed by a person needs none of your context: anyone on the team can follow it.** Cut ornament, never a reasoning step: a shorter text that makes the reader work out what you already knew is worse. Three things cost a reader an inference, so check for each by name: a pronoun or positional referent ("it", "here", "just below"), a conclusion without the step under it, and a contrast left to adjacency.

**The counterweight: the answer comes first, in a sentence or two, and everything after it is support the reader can skip.** Completeness goes after the answer, never in place of it.

Terser is fine between agents, which share the vocabulary, unless the message may be relayed to the user, which makes it text for a person. Session files, entry formats and commit bodies keep their own limits.

## Writing as though the reader can see your context

**A sentence's dependencies are in the reader's text, not only in your context.** Four tells, each checkable by someone who does not know the subject:

- **A reference resolvable only from a previous turn**: "the four", when the four were named a message ago.
- **A slot noun with no container**: "the item", when a roadmap, a checklist and a to-discuss list all have items. The same goes for entry, section, note, decision and question.
- **A label coined while designing**: "Rule B", "option 2". Restate the content every time.
- **A phrase whose disambiguator is in your head**: "grant access through a category" can mean using grants or creating them.

**A reference by position into something editable goes stale silently**: "the third row", "the section above". Name what the position holds.

**A decision put to the user carries its own context**: in this message and in plain words, what is being decided, what each answer would change, and your recommendation. Restate it in full while it stays open. **Ask only about decisions that block work now**: a prerequisite for later goes to the roadmap without a question.

**A question goes where the turn ends, and a progress update asks nothing.** Keep an update to one line on what is being done.

## Text that asks a person to act

**An error message, a prompt or an instruction gives one reading per word, raises no question it does not answer, and names the action rather than a state.** "Your sign-in was not accepted. Continue without signing in?" begs "why?", and "continue" reads as "retry". "Sign in again for a new token, or leave the token out to get the data available to everyone" does neither.

**Name a setting by what the reader types, not by its role.** "A filters option does not stand in for the Usher adapter" becomes "Leave filters out to use Usher's access control alone, or unset `ENABLE_ACCESS_ENFORCEMENT` to use those filters alone." `code-style.md` § What a user-facing error says covers what an error may reveal.

## Vocabulary drifts in two directions and conventions only watch one

- **One word, one sense; one sense, one word.** Overload is one word carrying several senses; synonymy is several words for one thing, and synonymy is the commoner one in text written piece by piece. Where what the word attaches to already makes the distinction, as in "the role's capabilities" and "the grant's capabilities", no second word is needed.
- **Name the whole class governed by a rule, not its most common member.** In an authorization text "user" reads as a human and silently drops service accounts: "access control decides what each principal may reach", not "what each user may reach".
- **Pin a generic word to one sense at first use, or replace it.** In "the host mounts the bridge", host could be the machine, the container or the application: write "the application embedding the bridge mounts it".
- **Moving senses off an overloaded word stops at a defined term**, which keeps its definition.
- **Reserve a term for a distinction that recurs**, and use a clause for one that is real but rare.
- **Test a new name on a reader with no context before adopting it**, by the five steps under this heading in `writing-style-detail.md`.

## Borrowing a register from a domain that is not the topic

- **The derivational register reads as ordinary English and can mean the opposite**: "falls out of", "what this buys", "turns on", "trivially", "by construction". Write "follows from", "what this gives", "depends on", and say why in words.
- **The borrowed-authority register claims obligations the work has not implemented**: "consent", "waiver", "governs" or "diagnosis" used loosely about a data model. Read the sentence as a practitioner of that field would; where it is not true for them, implement what the word promises or name the mechanism instead.

## Backwards-syntax clauses

**Never describe a noun with a clause that has its own noun subject.** "The records a category marks" makes the reader hold two noun phrases before the verb. Adding "that" does not fix it, since the order is the problem. It is worst where the verb can also read as a noun: `marks`, `holds`, `grants`, `covers`, `names`, `records`. Rewrite with a possessive, "the record's category", or a participle, "records marked by a category", or a compound, "category-marked records". "A tool for the client to build upon" counts too: "the client builds upon this tool". A personal pronoun as the clause's subject is fine, as in "the code it covers"; a quantifier pronoun is not, as in "the one category nothing marks".

A repair: "A saved-set filter or sort names only fields every reader is given" becomes "A saved-set filter or sort may name only fields given alike to every reader".

## Say it once, at the density it deserves

**State a fact once, in one form.** A caveat in prose and again as a bullet, or a condition explained and then given its own status label, costs the space of two. **The test: write the pair as one sentence.** If the result is absurd, the first half carried nothing. "Access is never granted over data directly. It is granted over a dataset." fails it; "Usher is not an authentication service" survives, because its negation carries weight.

## One derivation, one owner

**When two artifacts ship together and both could carry the same reasoning, the one read later on the reader's path owns it, and the earlier one keeps only enough to stand alone.** A commit body points at its changelog entry rather than re-deriving it.

**A sentence earns its place for the reader of that artifact.** A rule true only inside its derivation reads as noise elsewhere: "every value arrives exactly as given" belongs in the migration notes defining the field, not in the changelog.

## Naming the person you are talking to

- **Talking to the user, say "you", always.** A name in that slot reads as describing them to someone else who is present. **The test: if the name can be replaced by "you" and the sentence still works, write "you".** Never use both for the same person in one sentence. Use their name only where "you" does not fit: reasoning that narrates rather than addresses, or a sentence telling them apart from someone else.
- **In a shared, checked-in instruction file, say "the user"**: a name is wrong, since the file belongs to no one person, and "the developer" is wrong for the many who use agentics without being developers.
- **Where "user" means something in the project's own subject matter**, such as the account whose access is controlled, keep it for that meaning, and name another word for the person an agent works for in the project's `AGENTS.md`, such as "the developer".
- This governs live replies and visible reasoning. Persisted files name code, not people, per `AGENTS.md` § Critical constraints.

## Copy the shape, not the surface

**A model handed to you as a correction carries its author's habits along with the structure worth taking.** Adopt the structure, and run § Before anything leaves your control over the result exactly as over your own writing.

## Dashes

**Never use these in prose, conversational messages included**: em dashes (`—`, U+2014), en dashes (`–`, U+2013), double hyphens (`--`) standing in for a dash, and space-hyphen-space (` - `) as a sentence connector. Use a semicolon, a colon or a rewrite; in a heading, a colon: "OWASP Top 10: Quick Reference". In a bullet's inline annotation, `: ` separates: `` `path/to/file`: what changed ``.

Not prose, and fine: bullet markers (`-`), `---` dividers, table separator rows (`|---|---|`), compound-word hyphens (`well-designed`), numeric ranges (`1-2 entries`), code and arithmetic.

`grep -c '—\|–'` covers only two of the four forms, so check all four, or run the repository's own dash check where it covers all four. To fix a file: `sed -i '' 's/ — /: /g'`, then count again.

## No paragraph opens with "I"

**Never start a paragraph, a list item or a quoted line with "I" or a contraction of it**, in any output. The opening is where the point belongs. "I can't recall what the grant says on this" becomes "The grant may say something on this; I can't recall what."

## Spelling and language convention

A project sets its spelling convention in its `AGENTS.md` § This project's constraints, and the user can set their own in their global context. Example: Canadian English uses `-our` suffixes (colour, behaviour), `-re` suffixes (centre, fibre), `-ize` (not `-ise`), and `-yze` (analyze, paralyze; unlike the -ise/-ize split, Canadian does not diverge from American here).

## Language and typos

Flag typos and language issues wherever you spot them, in code, comments or documentation, rather than fixing them silently, so the user can decide. **An emoji is adornment, never meaning**: say a status, a warning or a severity in words.
