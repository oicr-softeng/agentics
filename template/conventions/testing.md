# Testing conventions

## Test file placement

Co-locate test files with the source they test: `validation.test.ts` in the same folder as `validation.ts`, not in a sibling `__tests__/` directory. A centralized `__tests__/` folder drifts further from the code it covers as the project grows.

When touching existing tests in `__tests__/` directories, note the inconsistency and log it as tech-debt.

## Test support code: fixtures, peripherals and placeholders

**Test support code, meaning code used by tests without being a test itself, comes in three kinds, and two are committed.**

- **Fixtures**: fixed data read by a test, such as records, configurations and expected outputs. Committed.
- **Peripherals**: lasting stand-ins for what a unit test should never need, such as a host's logger, a search engine or the clock. Committed.
- **Placeholders**: stand-ins for a component not yet built or not yet installable, such as an unpublished package. Never committed.

**To tell a peripheral from a placeholder, ask whether it is still needed once the real thing exists.** A peripheral is: a unit test should not need a real search engine even after one is installed. A placeholder is not: once its component is a dependency, the tests use the real one. The split is by lifetime. Fake, stub and mock name how a test double works, which is a separate question: a peripheral or a placeholder can be any of the three.

**Placeholders are throwaway work for their owner alone, so they stay out of every commit.** Committing one makes reviewers read, history keep and automation such as CI run code deleted once the real component arrives. List each placeholder in `.git/info/exclude` when you create it, along with each test importing it, so no `git add`, however broad, can pick them up. Keeping placeholders in `testing/placeholders/` lets one line exclude them all. A placeholder committed by mistake is removed by amending the unpushed commits holding it, since `git.md` treats unpushed commits as drafts. Seen in two repositories on one day, where removing placeholders after the fact meant rewriting 68 commits in one and 34 in the other.

**Record each placeholder in the agent's project memory, never in a committed file**: what it stands in for, the tests waiting on it, and the component they wait for. A committed record would describe files missing from every other clone, and the memory entry reminds a later session to move the tests once the component arrives.

**The costs are accepted, and written down so none is a surprise:**

- The tests needing a placeholder do not run in CI until the real component is a dependency.
- Excluded files live in one working tree. A fresh clone or worktree lacks them, and `git clean -x` deletes them. Losing a placeholder costs little; losing the tests waiting on it loses work.
- `git status` does not list excluded files, so the check in `git.md` § Unattributed working-tree changes cannot see them. Read `.git/info/exclude` for them instead.

**Once the real component is a dependency, the tests move onto it and are committed, and the placeholder is deleted.** A test failing against the real component marks a difference between the placeholder's assumed interface and the real one. Report it to the component's owner before changing either side, since the owner decides which one was wrong: directly, per `agent-index.md`, where `agent_index: yes`, and otherwise through the user.

**Every committed test passes on a clean checkout, at every commit.** Automation such as CI builds and tests the contents of a clean checkout and knows nothing else, so the committed tree has to stand on its own. A test stays uncommitted for one reason only: it needs a placeholder. A failing test is never left out to make the suite pass; fix it, or commit it together with the code making it pass. A failing test marked skipped or `todo` counts as left out. Where one stays anyway, it is technical debt, logged in `.dev/tech-debt.md` with its reason. Two checks: the committed suite passes in a fresh worktree, where excluded files are absent, as CI checks on every push; and every entry in `.git/info/exclude` is a placeholder or a test importing one. A fresh worktree checks the newest commit only. To check each commit on a branch, `git rebase --exec '<test command>' <base>` runs the suite at every one.

**Test support code lives under `testing/`, in folders named for what they hold**, such as `testing/fixtures/` and `testing/peripherals/`. Tests stay beside their source, per § Test file placement; only the support code they import moves under `testing/`. Support shared by more than one package goes in a private workspace package, marked `"private": true`, used as a development dependency through the package manager's workspace link (`workspace:*` where the package manager has that protocol), and never published.

## Plan-first workflow

For any feature or non-trivial change, follow this order:

1. **Plan**: present the approach and agree on it before writing anything
2. **Define behaviour**: write or describe the tests before implementation; tests are the specification
3. **Implement**: write code to make the tests pass

Designing tests first forces the interface to be thought through from the caller's perspective, surfacing architectural and API issues before they are baked into code.

**The tell, so this is checkable rather than asserted: in the history, does the test for a behaviour appear before or with the code implementing it, or after?** Tests written afterward describe what was built, which is the same defect as an expectation read off the implementation, one stage earlier. `git log --follow --oneline <test-file> <source-file>` answers it, and a reviewer who knows nothing about the feature can read the answer.

**Pragmatic exceptions:** Skip for pure structural/wiring work (type propagation, config plumbing, renaming) where a test would only verify "does this compile." BDD pays off most on logic with clear inputs and outputs: validation, transformations, business rules, utilities.

## BDD test style

Use `suite()` and `test()` with descriptive plain-language names that state the behaviour, `returns every header` rather than `excludes no headers`, per `code-style.md` § Naming conventions. Structure test bodies as setup → action → assertion (Given / When / Then). The specific testing framework is your team's choice; the naming and structure pattern applies regardless.

Example using Node's built-in `node:test` and `assert`:

```ts
import { suite, test } from 'node:test';
import assert from 'node:assert/strict';

suite('getNetworkPassthroughHeaders', () => {
  test('returns an empty array when no headers are configured', () => {
    const result = getNetworkPassthroughHeaders({ passthroughHeaders: [] });
    assert.deepEqual(result, []);
  });

  test('excludes empty string entries', () => {
    const result = getNetworkPassthroughHeaders({ passthroughHeaders: ['', 'Authorization'] });
    assert.deepEqual(result, ['Authorization']);
  });
});
```

- New tests: BDD style from the start
- Existing tests: rename and restructure when touching them in scope; large-scale rewrites belong in tech-debt

**Until a security fix is released, name its tests and suites for what the code must do, never for the case that failed or how it failed.** `rejects an archive that unpacks past the size limit` states the requirement, and appending `rather than checking only the outer archive` says which case failed and how. A contrast clause is the usual carrier, most often one built on `rather than`, `instead of` or `no longer`, while `without` and `still` often belong to the requirement itself, so each needs a reading rather than a deletion. The body has to exercise the failing input whatever the test is called, but the name travels without it, into CI output, the PR's list of checks and the tech-debt entry citing the test. See `security.md` § Releasing the fix for a hole carried by a published release.

## Rebuild `file:`-linked workspace dependencies before testing across a package boundary

A package referenced via `"file:../other-package"` resolves through that package's built output (`dist/`, per its `package.json`'s `main`/`exports`), not its live source. Editing that dependency's source and testing a consumer without rebuilding it first silently exercises stale compiled output. No error, no warning, just wrong behaviour.

Unit tests that import from a package's own source, via internal path aliases, within the same package, never cross this boundary and cannot catch it. Only a test that imports the dependency as a real consumer does, by package name, through its resolved entry point, will.

**Before trusting a "changed the dependency, tests still pass" result:** rebuild the dependency first, or write at least one test that exercises the real cross-package resolution path rather than importing from source.

## Prefer bare test-runner invocation over a shell-glob pattern in `test` scripts

`"test": "tsx --test ./src/**/*.test.ts"` (or the `node --test` equivalent) depends on the invoking shell expanding `**` recursively. `npm run <script>` executes through a non-interactive `sh`, which does not support globstar as an interactive bash/zsh session does.

Under plain `sh`, `**` behaves like a single `*` for one path segment: it silently matches only files at that exact depth, skipping everything shallower or deeper. No error, no warning. The only symptom is a lower-than-expected test or suite count, easy to miss unless someone happens to be watching for it.

**Fix:** use a bare `"test": "tsx --test"` (or `"node --test"`) with no path argument. The test runner's own recursive discovery isn't subject to the shell's globbing behaviour and finds every test file regardless of nesting depth.

## A test that builds its own input cannot discover the input it was never given

**The author of a test and the author of the code hold the same assumption about what the input looks like, so a test that constructs its input proves the code handles the shape it already expected.** The contract that matters is the one a real caller supplies, and it is exactly the one that never appears in the fixture. This is not a coverage problem: a line-coverage report can read as complete while the interesting case has never been constructed at all.

Instances in two repositories sharing no code:

- A suite exercising string handling where **no input contained a quote character**, so the escaping contract was never exercised at all. That code path exists for escaping.
- A suite exercising access control where **every test passed a null filter**, so nothing ever exercised a filter that filters. Every assertion passed and the feature under test was absent from all of them.
- A query builder whose `IN` clause construction was **never tested against a multi-element array**, so the clause's own case was the one never constructed.

**The failure is not a security-testing failure, and filing it as one is how it survives.** Two of the instances above look like security dimensions and the third is ordinary correctness, which is the point: a reader who files this under injection or access control will not apply it to a query builder.

**The check is one question per test double or fixture: what value would a real caller supply that I have not written here?** Then write that one. The high-yield answers are consistent across both cases above and worth walking deliberately: the value that needs escaping, the non-empty version of a filter, the absent field rather than the present one, the input that is already encoded, and the case produced by another component rather than the one you would type.

**A "passing" suite is evidence about the inputs it contains and nothing else.** When a test is meant to prove a transformation happens, assert the transformation, since a fixture needing no transformation makes an identity function pass the same test.

**Where the defect is an overwrite, the fixture has to make the two values differ.** A neighbour whose value already matches the one being written yields a passing test of nothing, because the wrong answer and the right answer coincide. In one reproduction a neighbour holding the same value hid the defect, and a neighbour holding a different value produced a corrupted line while the script still exited zero.

## Assert what the code should do, not what it does

The same family as the section above, seen from the other end: that one is about input never constructed, this one about an expectation copied out of the implementation.

**When writing a test against existing behaviour, state the requirement first and check the code against it.** An expectation read off the implementation passes permanently and certifies whatever is there, so fixing the defect then means editing the test that vouched for it. That moment is usually when anyone first notices the test was pinned to the behaviour rather than to the requirement.

**The tell is a justification that restates the mechanism.** `is healthy for an empty set of catalogues (nothing has failed)` explains how the code arrives at its answer instead of why that answer is right. Written from the requirement it reads *knowing nothing about the catalogues is not knowing they are fine*, which is the opposite assertion. `computeAggregateServerStatus({})` returned `HEALTHY` because no catalogues means none failed, a Kubernetes readiness probe reads it, and the effect is putting a replica that can serve nothing into rotation.

**The adversarial question is not "is this branch covered" but "if this assertion is wrong, what breaks, and would I notice?"** Coverage was complete in that case. The defect sat in the branch that occurred to the author first, which is what makes it worse than an untested branch rather than better: an untested branch is visibly absent, while a test asserting the wrong expectation is indistinguishable from a passing one and carries the authority of having been considered.

**It applies with most force to degenerate inputs.** Empty, zero, absent and unknown are where a permissive default hides, and where the code's own behaviour is most likely to be an accident rather than a decision.

**The rule is about the expectation's provenance, not its agreement with the code.** Pinning current behaviour is legitimate where the behaviour is the contract, as in a regression pin or a compiler-output snapshot, and a requirement-derived expectation that happens to match the implementation is fine. A second instance shows the family is not one defect: an assertion that a clause was present in a compiled query passed while being unable to distinguish a clause that restricts from one under `must_not` doing the opposite. Same shape, agreeing with the code and not testing the property claimed.

## A test ends when its work has ended

**Await all async work started by a test before the test ends, including work left in flight by a stop or close call.** Work that outlives its test runs during the next one, where it can act on that test's state, and the failure appears only when the two run in that order, so each test passes alone. Give a component's `stop()` or `close()` a promise that settles once its in-flight work has ended, and await it in every test or in the suite's teardown hook.

**Mock timers make the collision exact rather than probable.** Where a runner's mock timers restart their ids at each enable, as `node:test`'s have been observed to, a late callback clearing its own timer by id clears whichever timer of the next test now holds that id. A real timer handle is an object, so production code never collides this way, which is why the defect survives review: it exists only under the mocks.

## When to actually run the suite, not just write it

Run the full suite, not only the new tests, at each checkpoint under `definition-of-done.md` § Refinement passes: not just once at the end.
