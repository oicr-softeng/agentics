# Global context templates

These files are templates for your agent's **personal global context**: configuration that applies across all your projects, not just one.

Copy them to wherever your agent reads global/personal context:

| Agent | Global context directory |
|-------|--------------------------|
| Claude (Claude Code) | `~/.claude/` |
| Others | Consult your agent's documentation |

A project that adopts agentics reads `template/AGENTS.md` and the convention files under `conventions/` live, at the release pinned by its own `AGENTS.md`, and never copies them in. These templates are for having agentics' conventions across all your projects without adopting agentics into each one.

## Files

- `personal-preferences.md`: agent-neutral pointers to `template/AGENTS.md`'s interaction parameters and critical constraints, read fresh every session, plus the session-identity rules and the git trigger held inline, for a user who wants agentics' behavioural conventions personally without adopting it into any specific project
- `projects.md`: cross-project map: paths, memory locations, and relationships between your projects
- `roadmap.md`: template and convention for global roadmaps, including how to split by work context (professional vs personal)
- `security-guidelines.md`: OWASP-aligned security patterns, threat model, and code review triggers
- `agent-index.md`: opt-in bootstrap for the cross-project ownership registry and bulletin board (`conventions/agent-index.md` has the full design)

**A data file with an instructional block inside it goes stale in halves, and a keep-it-identical rule does not reach that.** `agent-index.md` is the case: the copy here is a schema, and a deployed copy is data instantiated from it, so the two are *required* to differ and a large diff between them is expected rather than drift. What goes stale is the embedded format block: a deployed copy whose block predates a schema field can hold entries in a shape not described by its own block, and a reader trusting the block over the convention concludes the shape is invalid. So when a schema field ships, the embedded block in every deployed copy is part of the change, on the same terms as the bootstrap copy in this repository.

## Bootstrapping without a project

When the user asks for personal, global-only setup (e.g. "set up my personal AI collaboration preferences using agentics, global context only"), don't run the project-level initialization flow at all, no `AGENTS.md`, no `.dev/`, nothing created or suggested for committing anywhere. First record where agentics lives, per agentics' `template/conventions/initialization.md` § Recording where agentics lives, then:

1. Ask the role question from `conventions/initialization.md` (developer / bioinformatician / AI engineering / general), and whether they're on the softeng team or working on an Overture project, skipping any already known from existing global context. Record each answer in global context, a no included, under its key in `conventions/initialization.md` § Setting up a project, so project setup does not ask again.
2. Read `personal-preferences.md` at the release tag `vX.Y.Z` of the newest release under agentics' `CHANGELOG.md` § Released, or of the newest release whose tag reads, and copy it into the user's own global context file (for Claude Code: merge its sections into `~/.claude/CLAUDE.md`, creating it if none exists yet, additive, don't overwrite unrelated existing content; for other agents: consult your agent's docs for where global/personal instructions live). Replace its first line, the version pointer `<!-- agentics-template-version: see CHANGELOG.md § Released (latest entry) -->`, with `<!-- agentics-template-version: X.Y.Z -->`, naming the release it was read at. Stamp no SHA: the version alone finds its release, so a SHA adds nothing.
3. Append the role file named beside the answer in the answer table of `conventions/initialization.md` § Setting up a project, and `AGENTS.softeng.md`/`AGENTS.overture.md` if applicable, to the same global context file, read at the same tag, once, per their answers. Each appended block keeps its own version tag at its top, stamped the same way, since the upstream check compares each copy separately.
4. Ask the propagation question from `conventions/initialization.md`, which says security notices come regardless and a yes adds notices of every other new release, and record the answer as `propagation_suggestions` in global context; nothing else needs asking.
5. Stop there. No project files, no `.dev/`, nothing to commit anywhere.
