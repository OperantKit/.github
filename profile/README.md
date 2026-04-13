# OperantKit

[日本語](README.ja.md)

Operant conditioning tools for behavioral research.

---

## Architecture

OperantKit separates concerns across five layers, distinguished by computational power and scope of responsibility:

```
                         contingency-dsl
 ┌─────────────────────────────────────────────────┐
 │  Core                 Non-Turing-complete (CFG)  │
 │  FR 10, Conc(VI 30-s, VI 60-s)                  │
 ├──────────────────────────────────────────────────┤
 │  Core-Stateful        CFG syntax, TC evaluation  │
 │  Pctl(IRT, 50), Adj(start=FR 1, step=2)         │
 ├──────────────────────────────────────────────────┤
 │  Core-TrialBased      CFG syntax, discrete trial │
 │  MTS(comparisons=3, consequence=CRF, ITI=5s)    │
 └──────────────────────────────────────────────────┘
 ┌──────────────────────────────────────────────────┐
 │  contingency-core     Turing-complete            │
 │  Dynamic transitions: phase changes, yoking,     │
 │  adaptive titration                              │
 ├──────────────────────────────────────────────────┤
 │  experiment-core      Turing-complete + safety   │
 │  Finalization: session termination, steady-state │
 │  criteria, ABA design verification               │
 └──────────────────────────────────────────────────┘
```

**contingency-dsl** declares *what* the contingency is.
**contingency-core** defines *how* contingencies change over time.
**experiment-core** ensures experiments *terminate and produce measurable data*.

Measurement specifications (response rate, reinforcement rate, IRT distributions, changeover rate) and their connection to procedure descriptions are the responsibility of the experiment and analysis layers -- not the DSL.

## Packages

| Category | Package | Role |
|---|---|---|
| **core** | contingency-dsl | Language-independent DSL specification (grammar, AST schema, conformance tests) |
| | contingency-dsl-py | Python reference parser |
| | contingency-dsl-paper | DSL AST &rarr; JEAB/J-ABA Method section compiler |
| | contingency-dsl-reader | Method section text &rarr; DSL AST extractor |
| | contingency-py | Python reinforcement schedule engine |
| | contingency-rs | Rust implementation (HIL, PyO3/WASM/C FFI bindings) |
| | models | Core behavioral models (methods & results) |
| **experiment** | experiment-core | Session lifecycle, experimental context, renewal paradigms |
| | contingency-annotator | Annotation extensions (stimulus, temporal, subjects, apparatus) |
| | session-recorder | Session runner + JSONL event logger |
| **analysis** | session-analyzer | Cumulative records, statistics, model fitting |
| **simulators** | behavior-simulator | Animal model simulation |
| | ai-operant-box | LLM as virtual subject |
| | social-contingency-sim | Social contingency simulator (7 strategies x 5 scenarios) |
| **tools** | aba-advisor | ABA intervention advisor (TF-IDF + rules + opt-in LLM) |
| | behavior-scope | Non-invasive real-time observation dashboard |

## Origins

Reviving and evolving [YutoMizutani/OperantKit](https://github.com/YutoMizutani/OperantKit) (Swift, MIT, 2018-2020) as a modern behavioral analysis toolkit.
