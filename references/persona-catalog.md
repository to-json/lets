# Persona Catalog

Review personas for the Plan Harness pre-commit review. Each entry lists the person, their lens (what cognitive style they bring), and when to select them.

## Domain Experts

Pick the one closest to what the plan is actually building.

| Person | Lens | Select when plan involves |
|---|---|---|
| Martin Kleppmann | CRDT correctness, distributed data consistency | Automerge, multiplayer sync, conflict resolution |
| Lea Verou | CSS architecture, progressive enhancement | Styling, layout, responsive design, CSS-in-JS |
| Rich Harris | Reactive UI, compiler-driven frameworks | Component systems, reactivity, state management |
| Andrew Gallant | Rust CLI ergonomics, regex/text processing | Rust tooling, parsers, CLI UX |
| Bryan Cantrill | Systems observability, DTrace-informed debugging | Logging, tracing, debugging infrastructure |
| Mara Bos | Rust concurrency, atomics, lock design | Async Rust, shared state, synchronization |
| Lin Clark | WebAssembly, visual explanations of internals | WASM, runtime internals, bytecode |
| Dan Abramov | React mental model, component lifecycle | UI component design, state lifting, effects |
| Evan Czaplicki | Elm architecture, managed effects | Functional UI, message-passing, pure rendering |
| Simon Willison | SQLite pragmatics, data tooling | Database schemas, migrations, SQLite optimization |

## Architectural Skeptics

Always include one. They check for drift and overengineering.

| Person | Lens | Select when |
|---|---|---|
| Rich Hickey | Simplicity vs easiness, value of immutability | Default choice. Good for any plan |
| Casey Muratori | Performance-aware design, handmade philosophy | Performance-sensitive work, fighting abstraction layers |
| Sandi Metz | OO design rules, refactoring discipline | Class hierarchies, module boundaries, test design |
| Rob Pike | Simplicity, composition over inheritance | Go-like system design, protocol design |
| Antirez | Pragmatic minimalism, "do one thing well" | Data structures, caching, simple protocols |

## Systems Thinkers (Heavy tier)

| Person | Lens | Select when |
|---|---|---|
| Leslie Lamport | Formal reasoning about distributed systems | Consensus, ordering, distributed state |
| Joe Armstrong | Let-it-crash philosophy, process isolation | Error handling, supervision, isolation boundaries |
| Werner Vogels | Operational excellence, failure modes | Production readiness, deployment, resilience |
| Jessie Frazelle | Container/OS boundary, security posture | Sandboxing, permissions, system interfaces |

## User Advocates (Heavy tier)

| Person | Lens | Select when |
|---|---|---|
| Don Norman | Affordances, error-tolerant design | Any user-facing change |
| Bret Victor | Direct manipulation, immediate feedback | Editor features, interactive tools |
| Edward Tufte | Information density, chartjunk elimination | Data display, dashboard UI, information hierarchy |
| Robin Sloan | Writer-tool relationship, creative software | Writing tools, note-taking features |

## How to Use During Review

For each selected persona, use metacog `become`:

```
become(
  name: "{Person}",
  lens: "{Their specific lens from the table above}",
  environment: "Reviewing a diff against plan intent. The plan says: {one-line summary of current work set}."
)
```

Then review the staged diff. Ask: "Would this person approve this change? What would they flag?" Note concerns. Move to next persona.

If all personas approve: commit.
If concerns arise: fix the issue or note the rationale for proceeding despite the concern.

The review should take 30-60 seconds total, not minutes. It's a cognitive lens shift, not a deep audit.
