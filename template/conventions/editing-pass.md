# Editing pass for a document other people will read

Read this when a document is finished and someone other than you will read it: documentation, a design note, a README or a PR description. `writing-style.md` holds the rules every output follows; this file is the order to apply them in, the checks that catch what reading alone misses, and the rules that apply to documents only.

## The passes, in order

1. **Audience and next action.** Name who reads it and what they do afterwards, near the top and before the content, where a reader decides whether the document is for them: "for an administrator who has not used the identity provider or seen this repository".
2. **Ground.** Read every source the claims rest on, in this session. A claim with no line to point at is an inference: keep a list of those. Where two sources disagree, report the disagreement rather than settling it in the edit, and where the writer's intent is unclear, keep the wording and flag it; a guessed intent or a side picked between sources is where an edit turns destructive.
3. **Shape.** Table, list, numbered trace or prose, by the number and kind of items, per `writing-style.md` § Prose is a serialization, and sections in the order of the reader's task. Two checks hold at section level. **A section uses nothing defined after it:** for each concept used in a section, its definition sits above it, and where the task order and the definition order disagree, the definition moves earlier rather than the text pointing ahead; a page describing a data mapping and then the policy built on it, whose policy section defined the terms used by the mapping, swaps the two and moves the shared definitions into the first. **A section holds only what its heading promises:** how the system treats the data belongs in the section about the system, not in the one describing the data.
4. **Draft, point first**, per `writing-style.md` § Deliver the point first.
5. **Terms.** Check each against the project's glossary and its list of retired terms, and each identifier against its description. Explain each term once, at first use, in plain words, in a sentence of its own.
6. **Claims.** Check every universal and every count against the source, separately from the rest of its claim: "every", "all", "only", "never", "each", "a few", a number, a superlative. Then check every causal link: "so", "because", "which is why". Fluent text overreaches in exactly these words, because a sentence reads as more useful with them, and an invented causal link is among the most damaging errors, because a reader acts on a stated cause.
7. **A cold read as the named reader**, for unresolved pointers, slot nouns, coined labels and missing reasons, per `writing-style.md` § Writing as though the reader can see your context.
8. **Status and process words**, per `writing-style.md` § Say what changed, not how you got there.
9. **The mechanical checks, last, on the exact final text.**
10. **An independent read, then the owner.** A reader outside the session that edited the document reads the result as its named reader, for any correct meaning changed, any sentence made worse and any repetition, since a review inside that session passes what an outside reader catches. Then it goes to the owner with the inference list, so the claims inferred rather than read get checked first.

**Every addition carries its weight.** The passes add more than they remove, since defining terms, leading with the threat and naming what a finding depends on all add words. So each added clause carries a reason, a limit or a definition the reader needs, a definition or fact already in the document is pointed at rather than repeated.

**What sends you back a pass.** A correction to one instance means re-checking the whole class in the document, not that instance alone. A tool that rewrote the file, such as a formatter, means re-reading it before the next edit. A changed fact means searching the whole corpus for its topic, since sentences outside the section you were asked to change may rest on the old state.

## Mechanical checks

- The four banned dash forms, with the count reported.
- Absolute local paths, such as `/Users/` or `/home/`.
- Spellings against the project's convention in `writing-style.md` § Spelling and language convention; under an `-ize` convention, a search for `-ise` forms, minus root words such as advise, revise and surprise.
- The project's formatter, where it has one, then a re-read.
- The project's retired terms. A retired term is a fixed list and so a search; whether a term is defined needs a reader.
- Status words: "yet", "planned", "still", "once deployed", "TODO", "for now".
- Every file name, path and identifier exists, seen in this session rather than taken from a list supplied by someone else.
- Every linked heading exists, and is a heading rather than bold text.
- No names or private URLs in the diff, since tickets and attached documents carry both.

**Nothing mechanical catches a false statement.** The dash and spelling checks catch the most by count, at low stakes; the claims pass and the topic search catch the most by stakes; and the inference list sent to a verifier is the cheapest check with a high yield.

## What drafts most often get wrong

Most frequent first:

1. Terms of the writer's own field, used undefined for a reader outside it.
2. Security text led by mechanism, saying what a control does rather than which threat it stops.
3. Status narrative.
4. Coined labels and positional references.
5. Universals and invented causal links.
6. An identifier in prose where a description belongs.
7. A claim fused with its qualification.
8. Attribution where the reason should be.

Items 4 and 5 are as common in a strong writer's drafts as in anyone's, which is why the passes check them rather than trusting the writer.

## Rules for documents

- **A Why cell gives the reason first, in plain words, and defines nothing.** Lead with what the change prevents or makes possible, then the mechanism, and move every definition to its first use elsewhere.
- **Prose names a thing by what it is, and its identifier appears only where the reader types, matches or searches for it.** "The administration realm" in prose, `master` in configuration, URLs and commands, with a terms section saying the two are one thing.
- **A component whose short name is a generic word in the same domain keeps its qualifier.** "The GraphQL router", never "the router", which reads as any router.
- **A finding records what it is contingent on, in the same sentence:** the version measured against, the assumption held, or the change whose landing would alter it. Written flat, nothing in the text says what would make it false.
- **Say which layer a statement describes: the model, or what a release implements.** A first release's limit of one category per record, written flat, reads as the model's rule, when the model allows any set of categories.
- **A list lives where a copy of the listed things carries it.** A module's file index belongs in a README inside the module, so it travels with a copy of the directory, and every other document points at it.
- **For a newcomer, consider three devices, as the document needs them.** A terms table with the columns "Term", "What it is" and "Example here", tying each term to this deployment's own instance; a tree of the parts; and each mechanism as a numbered trace of one concrete case, so a reader can disagree with one step.

## A change of course is stated as one

**A change to something settled says so in one plain sentence, naming what a person could do before and cannot after.** "The plan let each person choose their model endpoint; this changes it so the deployment chooses" is that sentence. A description of the new state alone, however accurate, passes review without anyone seeing that it reverses the old one. Confirm the change with the user before writing it, per agentics' `template/AGENTS.md` § Interaction parameters; this section is about the text that carries it afterwards.

These moves hide a change of course, and each can be checked for by name, when writing a change and when reviewing one. Check against the default branch, never against the change's own previous state: a change of course is defined against what the default branch records, and a review comparing one commit with the last sees only the latest step, so it can push a redirect further while trying to correct it.

- **A settled question retitled**, so that it reads as a new open question.
- **One change cited as an existing fact** to justify another made in the same change.
- **A change placed inside a bullet about something else**, such as who chooses a model inside a bullet about where tokens are kept.
- **A removal framed as a fix**, so that taking a choice away reads as closing a hole.
- **A question marked decided without naming what settled it**, so that a reader of the document alone cannot tell a rule the user stated from the author's own answer.
- **A heading and its body that disagree about who acts.** This shows that one of them changed, not which one: check both against the default branch before making either match the other, since matching the wrong one extends the change.

## Judgment, by example

- **Which half of a two-claim sentence keeps its place.** Keep the actionable half. A recovery step carried a definition, "the browser flow is the sequence of steps run by the identity provider when someone signs in through a browser", inside an instruction about what to configure; the definition became a row in the terms table, and the step kept only what to do.
- **An instruction read for its purpose.** "No table of files left in the setup document" serves a single index. So a table of kinds of settings stays, its rows naming one file each collapse into one row pointing at the directory, and each such judgement is reported to whoever gave the instruction.
- **Density by topic, not by reader.** The same reader can take dense text on a subject they know and need a concrete example first on one they do not.
- **Define a near-synonym before anyone argues about it.** Where a team will ask "why not just call it X?", the distinction goes in the glossary before the question arrives.
