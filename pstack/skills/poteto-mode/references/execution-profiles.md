# Execution profiles

Execution profiles bound workflow cost without weakening the correctness requirements that match the risk. They do not select models or reasoning effort. `/setup-pstack` configures those separately.

Resolve one profile before the first delegation. Explicit operator instructions win. Otherwise read `# execution profile:` from `~/.cursor/rules/pstack-models.mdc`. Missing configuration means `balanced`.

Every agent count is recursive. A nested reviewer or verifier counts exactly like a root-level owner. Deterministic shell commands, tests, linters, and existing CI do not count as agents.

## Lean

Use for local work, limited credits, small programs, and ordinary product changes.

- Run at most 2 subagents concurrently and 6 total.
- Use at most 1 planning explorer. The coordinator may inspect the repository directly instead.
- Do not allow nested subagents. An owner writes and verifies its own unit.
- Keep the same owner for implementation and one fix round.
- Verify with targeted tests and typecheck or lint. Add one live scenario when the change affects a user-facing interaction, persistence, or an external boundary.
- Use one independent review only for auth, money, destructive migrations, sensitive data, or concurrency.
- Run the full suite once before handoff when its cost is reasonable. Do not run it after every fix.
- Measure performance only when it is an acceptance criterion or the change touches a demonstrated hot path.
- Record unrelated findings. Do not implement them unless they block the requested work.
- Use event-driven monitoring where available. Otherwise audit no more often than every 3 hours, and do not wake a model when deterministic status is unchanged.

## Balanced

Use by default. It preserves independent review and real-surface proof while bounding repeated work.

- Run at most 3 subagents concurrently and 12 total.
- Use at most 2 planning explorers, divided by genuinely independent subsystems. Do not add a separate plan reviewer when the deterministic plan checker passes.
- Permit one nested subagent per owner at a time. Nested agents count toward both limits.
- Keep the same owner through implementation and its first fix round. A later round uses a fresh owner only when the remaining issue needs independent judgment or the original context is degraded.
- Scale live verification by risk. Use 1 lane for ordinary changes and up to 3 lanes for auth, money, destructive migrations, sensitive data, concurrency, or a cross-system send.
- Use one independent audit for ordinary changes and two focused audits for high-risk changes.
- Run the full suite once per dependency wave or final stack. After a fix, rerun affected tests and lanes plus one smoke check. Reuse unaffected evidence.
- Measure performance only when it is an acceptance criterion, the diff changes a known hot path, or a regression signal exists.
- Record out-of-scope findings in the decision trail. Stop and ask before implementing them unless they block the requested result. A critical security finding gets an immediate concise report, not an expanding audit or hotfix program.
- Prefer event-driven monitoring. When the platform supports a deterministic preflight, skip the model turn when nothing changed. Otherwise audit active programs no more often than every 2 hours, probe status before rereading context, and end immediately when nothing changed.

## Maximum rigor

Use only when the operator explicitly requests `maximum-rigor`, when policy requires exhaustive evidence, or when the cost of a missed defect clearly exceeds the run's compute budget.

- Use the playbook's full owner, swarm, fresh-agent, live-lane, perf, and audit requirements.
- Ten live lanes, multiple independent audits, a fresh verdict after each changed patch, and hourly audit ticks are allowed.
- State why maximum rigor is warranted before fan-out begins.

## Shared stop rules

- Existing branches are inputs. Audit and integrate them. Rebuild only when a failing check proves the existing implementation is inadequate.
- One review round should return all findings together. Do not drip findings into serial fix agents.
- After two fix rounds on one unit, stop spawning agents. Reassess the design or report the blocker.
- On a usage-limit warning, stop new launches, tell running children to checkpoint, and persist every dirty worktree before continuing.
- Keep agent reports concise. Put detailed evidence in files and return paths plus decisions, not long transcript dumps.
