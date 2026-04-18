# OperantKit

:jp: [日本語版 README](README.ja.md)

Operant conditioning tools for behavioral research.

---

## Architecture

The core package **contingency-dsl** is a language-independent specification for declaring reinforcement contingencies and Pavlovian pairings. It is organized into six layers by scientific category under a paradigm-neutral formal foundation:

```
 ┌──────────────────────────────────────────────────────────────┐
 │  Annotation    JEAB Method metadata (Subjects / Apparatus /  │
 │                Procedure / Measurement) + extensions         │
 ├──────────────────────────────────────────────────────────────┤
 │  Experiment    Multi-phase designs; phase & context as       │
 │                first-class; annotation inheritance           │
 ├──────────────────────────────────────────────────────────────┤
 │  Composed      Operant × Respondent: CER, PIT, autoshaping,  │
 │                omission, two-process theory                  │
 ├──────────────────────────────┬───────────────────────────────┤
 │  Operant                     │  Respondent                   │
 │  Three-term contingency      │  Two-term contingency         │
 │  (SD-R-SR); Ferster-Skinner  │  (CS-US); Tier A primitives   │
 │  schedules; stateful;        │  (Pair, Contingency,          │
 │  trial-based; aversive       │  Compound, Differential, …)   │
 ├──────────────────────────────┴───────────────────────────────┤
 │  Foundations   CFG / LL(2) meta-grammar; paradigm-neutral    │
 │                types (contingency, time, stimulus, valence)  │
 └──────────────────────────────────────────────────────────────┘
```

**Foundations** provides paradigm-neutral lexical and type structure.
**Operant** and **Respondent** declare *what* each contingency is.
**Composed** expresses procedures that combine both paradigms as `PhaseSequence` AST trees built from operant + respondent primitives.
**Experiment** declares multi-phase designs with phase and context as first-class constructs.
**Annotation** attaches JEAB Method-category metadata to any construct.

The base DSL is non-Turing-complete (CFG). Deeper Pavlovian procedures (higher-order conditioning, blocking, occasion setting, renewal, reinstatement, etc.) live in the companion package **contingency-respondent-dsl**, which plugs into the Respondent extension point.

Measurement specifications (response rate, reinforcement rate, IRT distributions, changeover rate) and their connection to procedure descriptions are the responsibility of the experiment and analysis layers — not the DSL.

## Packages

| Category | Package | Role |
|---|---|---|
| **core** | contingency-dsl | Language-independent DSL specification (EBNF, AST schema, conformance tests) |
| | contingency-respondent-dsl | Tier B Pavlovian procedures extending the Respondent layer |
| | contingency-dsl-py | Python reference parser |
| | contingency-dsl2procedure | DSL AST &rarr; JEAB/J-ABA Method section compiler |
| | contingency-procedure2dsl | Method section text &rarr; DSL AST extractor |
| | contingency-py | Python reinforcement schedule engine |
| | contingency-rs | Rust engine (PyO3 / WASM / C FFI / KMP bindings, HIL binary) |
| | models | Core behavioral models (methods & results) |
| **experiment** | experiment-core | Session lifecycle, experimental context, renewal paradigms |
| | stimulus-annotator | Annotation reference implementation (stimulus, temporal, subjects, apparatus) |
| | session-recorder | Session runner + JSONL event logger |
| **experiment-tools** | experiment-io | Hardware I/O wrapper over `contingency.hw` |
| | contingency-bench | HIL timing-precision benchmark harness |
| | schedule-writer / schedule-visualizer | DSL authoring & visualization tools |
| **analysis** | session-analyzer | Cumulative records, statistics, model fitting |
| **simulators** | behavior-simulator | Animal model simulation |
| | ai-operant-box | LLM as virtual subject |
| | social-contingency-sim | Social contingency simulator (7 strategies × 5 scenarios) |
| **tools** | aba-advisor | ABA intervention advisor (TF-IDF + rules + opt-in LLM) |
| | behavior-scope | Non-invasive real-time observation dashboard |
| | operantkit-frontend | Experiment design UI |

## Origins

Reviving and evolving [YutoMizutani/OperantKit](https://github.com/YutoMizutani/OperantKit) (Swift, MIT, 2018–2020) as a modern behavioral analysis toolkit.
