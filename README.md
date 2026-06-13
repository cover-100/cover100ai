# cover100

**100% test coverage — or a documented reason why not.**

Website: [cover100.dev](https://cover100.dev/)

cover100 is a Claude Code plugin that drives a repository to fully verified
statement coverage with a multi-agent harness: every generated test is gated by
an independent safety verifier, every branch that genuinely can't be covered is
documented with the refactor it would need, and an interrupted run never loses
finished work.

```
/cover100 ./pkg/auth/...
```

## The report never lies to you

Every package ends a run in exactly one of three states — and each carries its receipts:

- **100% verified** — fully covered, with tests an independent verifier accepted:
  they build, they pass, they assert real behavior. Coverage-padding tests
  (`_ = result`, calls without checks) are rejected, not counted.
- **Documented gap** — a branch that genuinely can't be covered without
  refactoring gets an entry in the package's `TEST-COVERAGE.md`: why it's
  un-coverable, and the exact refactor that would unlock it. Settled gaps are
  never silently re-fought on the next run, and never silently dropped.
- **Quarantined** — packages that don't build, generated code, and vendored
  trees are listed with reasons and excluded from the claim. The tool degrades by
  telling you, never by pretending.

## How it works

A deterministic pipeline of specialized agents — not one heroic prompt:

1. **Collect** — a deterministic script measures the statement-coverage baseline
   and triages the work: already-covered, settled-gap and broken packages are
   skipped with zero spend.
2. **Research** — researchers classify *why* each region is uncovered (error
   path, time/random, external I/O, defensive code) and prescribe the concrete
   testing approach before any test is written.
3. **Engineer** — parallel engineers work in isolated git worktrees, one unit
   each, committing every package the moment it's green. Shared fakes live in a
   keyed knowledge base so the same mock is never invented twice.
4. **Verify** — an independent safety verifier gates every unit: build green,
   assertions meaningful, production changes limited to declared seams, no new
   lint findings. Unverified work doesn't merge.
5. **Integrate & report** — serialized merges, a whole-module finalize pass, and
   a portable `REPORT.md`: per-package table, gap register, dependency list, and
   instructions the *next* run consumes automatically.

**No work lost. Ever.** Abort a run mid-flight, lose your connection, kill an
agent — finished packages are already committed, interrupted units are parked,
and the next run salvages everything that builds and passes. Resuming is the
default motion, not a recovery procedure.

## Two modes

- **drive-to-100** — take a subtree or a whole repository to the claim above.
  Start small (`/cover100 ./pkg/auth/...`) with a cost projection up front;
  whole-repo runs scale the same harness with budget controls, model escalation
  for stubborn packages, and cross-run memory.
- **rescan** — audits *existing* test suites: hand-rolled fakes that duplicate
  the standard library, copy-paste test functions that should be one table test,
  dead helpers. Then refactors them — coverage may not drop, assertions may not
  weaken, test code should shrink.

## Languages

- **Go** — statement coverage, seam-only production policy
- **TypeScript / JavaScript** — vitest & jest, tests-only policy
- **Python** — next
- **.NET & Java** — roadmap

## Covered with cover100

Projects driven toward verified 100% coverage with cover100:

- [specscore/specscore-cli](https://github.com/specscore/specscore-cli) — CLI for SpecScore: lint, query, and scaffold specifications.
- [dal-go/dalgo](https://github.com/dal-go/dalgo) — database abstraction layer for Go applications.
- [ingitdb/ingitdb-cli](https://github.com/ingitdb/ingitdb-cli) — CLI for inGitDB, a schema-validated database whose storage engine is a Git repository.
- [bots-go-framework/bots-fw](https://github.com/bots-go-framework/bots-fw) — Go framework for developing bots for messengers.

## Install

### Claude Code

```
/plugin marketplace add specscore/ai-marketplace
/plugin install cover100@cover-100
```

### Manual

```sh
git clone https://github.com/cover-100/cover100ai
cp -r cover100ai/skills ~/.claude/skills/
```

## Family

cover100 is built and dogfooded by the [SpecScore](https://specscore.md/)
project — spec-driven development where intent is machine-readable and tests are
the executable half of the spec.

## License

MIT
