# OperantKit

:gb: [English README](README.md)

行動研究のためのオペラント条件づけツールキット。

---

## アーキテクチャ

中核パッケージ **contingency-dsl** は、強化随伴性と Pavlov 型対提示を宣言するための言語非依存な仕様である。パラダイム中立な形式的基盤の上で、科学的カテゴリ別に 6 層で構成されている：

```
 ┌──────────────────────────────────────────────────────────────┐
 │  Annotation    JEAB Method メタデータ（Subjects / Apparatus / │
 │                Procedure / Measurement）+ 拡張                │
 ├──────────────────────────────────────────────────────────────┤
 │  Experiment    多フェーズデザイン; phase と context を        │
 │                第一級構成要素; アノテーション継承              │
 ├──────────────────────────────────────────────────────────────┤
 │  Composed      Operant × Respondent: CER, PIT, autoshaping,   │
 │                omission, two-process theory                   │
 ├──────────────────────────────┬───────────────────────────────┤
 │  Operant                     │  Respondent                   │
 │  三項随伴性 (SD-R-SR)         │  二項随伴性 (CS-US)            │
 │  Ferster-Skinner 分類の       │  Tier A プリミティブ           │
 │  スケジュール; 状態保持;       │  (Pair, Contingency,          │
 │  試行ベース; 嫌悪制御          │  Compound, Differential, …)   │
 ├──────────────────────────────┴───────────────────────────────┤
 │  Foundations   CFG / LL(2) メタ文法; パラダイム中立な型       │
 │                (contingency, 時間, 刺激, valence)             │
 └──────────────────────────────────────────────────────────────┘
```

**Foundations** はパラダイム中立な字句と型の構造を提供する。
**Operant** と **Respondent** は各随伴性が「何であるか」を宣言する。
**Composed** は両パラダイムを組み合わせる手続きを、operant + respondent プリミティブから構成した `PhaseSequence` AST ツリーとして表現する。
**Experiment** は phase と context を第一級構成要素として多フェーズデザインを宣言する。
**Annotation** は JEAB Method カテゴリ準拠のメタデータを任意の構成要素に付与する。

基底 DSL は非チューリング完全（CFG）である。より深い Pavlov 型手続き（高次条件づけ、阻止、条件性弁別（occasion setting）、更新（renewal）、再生（reinstatement）等）は姉妹パッケージ **contingency-respondent-dsl** に存在し、Respondent 拡張点に差し込まれる。

測定仕様（反応率、強化率、IRT 分布、切り替え率）と手続き記述との接続は、実験層・分析層の責務であり、DSL の責務ではない。

## パッケージ

| カテゴリ | パッケージ | 役割 |
|---|---|---|
| **core** | contingency-dsl | 言語非依存の DSL 仕様（EBNF, AST スキーマ, 適合テスト） |
| | contingency-respondent-dsl | Respondent 層を拡張する Tier B Pavlov 型手続き |
| | contingency-dsl-py | Python リファレンスパーサ |
| | contingency-dsl2procedure | DSL AST → JEAB/J-ABA Method section コンパイラ |
| | contingency-procedure2dsl | 論文 Method section → DSL AST 抽出器 |
| | contingency-py | Python 強化スケジュールエンジン |
| | contingency-rs | Rust エンジン（PyO3 / WASM / C FFI / KMP バインディング、HIL バイナリ） |
| | models | コア行動モデル（手続き・結果） |
| **experiment** | experiment-core | セッションライフサイクル、実験文脈、復活パラダイム |
| | stimulus-annotator | アノテーション参照実装（刺激、時間、被験体、装置） |
| | session-recorder | セッションランナー + JSONL イベントロガー |
| **experiment-tools** | experiment-io | `contingency.hw` を包むハードウェア I/O ラッパー |
| | contingency-bench | HIL タイミング精度ベンチマークハーネス |
| | schedule-writer / schedule-visualizer | DSL 作成・可視化ツール |
| **analysis** | session-analyzer | 累積記録、統計分析、モデルフィッティング |
| **simulators** | behavior-simulator | 動物モデルシミュレーション |
| | ai-operant-box | LLM を仮想被験体とする実験環境 |
| | social-contingency-sim | 社会的随伴性シミュレータ（7 方策 × 5 シナリオ） |
| **tools** | aba-advisor | ABA 介入提案エンジン（TF-IDF + ルール + opt-in LLM） |
| | behavior-scope | 非侵襲的リアルタイム観測ダッシュボード |
| | operantkit-frontend | 実験設計 UI |

## 起源

[YutoMizutani/OperantKit](https://github.com/YutoMizutani/OperantKit)（Swift, MIT, 2018–2020）を復興・進化させた行動分析ツールキット。
